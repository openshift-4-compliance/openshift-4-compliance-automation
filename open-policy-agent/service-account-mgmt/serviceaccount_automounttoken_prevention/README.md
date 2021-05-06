# Policy Explainer & Testing - ServiceAccount Automount Token Prevention

The policy disallows creating serviceAccounts that does not include the following key:value pair in its manifest: `"automountServiceAccountToken": false`

It is useful (security-wise) so that by default services' pods won't be able to mount the token of the serviceAccount which deployed them if they aren't meant to. In case that a serviceAccount token is required it can be excluded.

## Deploy the template & constraint yamls that define the policy

```bash
cat > constraint.yaml << EOF
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sServiceAccountAutomountTokenPrevention
metadata:
  name: serviceaccount-automounttoken-prevention
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["ServiceAccount"]
    excludedNamespaces: 
      - hive
      - kube-node-lease
      - kube-public
      - kube-storage-version-migrator-operator
      - kube-system
      - open-cluster-management
      - open-cluster-management-hub
      - open-cluster-management-agent
      - open-cluster-management-agent-addon
      - openshift
      - openshift-apiserver
      - openshift-apiserver-operator
      - openshift-authentication
      - openshift-authentication-operator
      - openshift-cloud-credential-operator
      - openshift-cluster-csi-drivers
      - openshift-cluster-machine-approver
      - openshift-cluster-node-tuning-operator
      - openshift-cluster-samples-operator
      - openshift-cluster-storage-operator
      - openshift-cluster-version
      - openshift-compliance
      - openshift-config
      - openshift-config-managed
      - openshift-config-operator
      - openshift-console
      - openshift-console-operator
      - openshift-console-user-settings
      - openshift-controller-manager
      - openshift-controller-manager-operator
      - openshift-dns
      - openshift-dns-operator
      - openshift-etcd
      - openshift-etcd-operator
      - openshift-gatekeeper-system
      - openshift-image-registry
      - openshift-infra
      - openshift-ingress
      - openshift-ingress-canary
      - openshift-ingress-operator
      - openshift-insights
      - openshift-kni-infra
      - openshift-kube-apiserver
      - openshift-kube-apiserver-operator
      - openshift-kube-controller-manager
      - openshift-kube-controller-manager-operator
      - openshift-kube-scheduler
      - openshift-kube-scheduler-operator
      - openshift-kube-storage-version-migrator
      - openshift-kube-storage-version-migrator-operator
      - openshift-kubevirt-infra
      - openshift-machine-api
      - openshift-machine-config-operator
      - openshift-marketplace
      - openshift-monitoring
      - openshift-multus
      - openshift-network-diagnostics
      - openshift-network-operator
      - openshift-node
      - openshift-oauth-apiserver
      - openshift-openstack-infra
      - openshift-operators
      - openshift-operator-lifecycle-manager
      - openshift-ovirt-infra
      - openshift-ovn-kubernetes
      - openshift-sdn
      - openshift-service-ca
      - openshift-service-ca-operator
      - openshift-user-workload-monitoring
      - openshift-vsphere-infra
EOF

cat > template.yaml << EOF
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8sserviceaccountautomounttokenprevention
  annotations:
    description: Checks wheter the pod has a unique serviceAccount, correlated with the pod's name
spec:
  crd:
    spec:
      names:
        kind: K8sServiceAccountAutomountTokenPrevention
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package K8sServiceAccountAutomountTokenPrevention

        violation[{"msg": msg}] {
          not has_key(input.review.object, "automountServiceAccountToken")
          msg := "The key automountServiceAccountToken should exist in your serviceAccount object"
        }
        
        violation[{"msg": msg}]{
        	input.review.object["automountServiceAccountToken"] != false
          msg := "The automountServiceAccountToken key in your serviceAccount object MUST have the value of false"
        }

        has_key(x, k) { _ = x[k] }
EOF

oc create -f template.yaml -n openshift-gatekeeper-system
oc create -f constraint.yaml -n openshift-gatekeeper-system

oc describe constrainttemplate/k8sserviceaccountautomounttokenprevention
oc describe k8sserviceaccountautomounttokenprevention
```

## Deploy test pods & deployment object to make sure the policy works as expected
```bash

oc new-project test

# No automountServiceAccountToken <=> not approved 
cat > service_account_should_not_work_basic.yaml << EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: example
EOF

# expect failure
oc create -f service_account_should_not_work_basic.yaml -n test

# automountServiceAccountToken != false <=> not approved 
cat > service_account_should_not_work_advanced.yaml << EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: example
automountServiceAccountToken: true
EOF

# automountServiceAccountToken == false <=> approved
oc create -f service_account_should_not_work_advanced.yaml -n test

cat > service_account_should_work.yaml << EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: example
automountServiceAccountToken: false
EOF

# expect success
oc create -f service_account_should_work.yaml -n test

## Test namespace excluding ##
# expect success
oc create -f service_account_should_not_work_basic.yaml -n openshift-operators
oc delete -f service_account_should_not_work_basic.yaml -n openshift-operators
oc create -f service_account_should_not_work_advanced.yaml -n openshift-operators
oc delete -f service_account_should_not_work_advanced.yaml -n openshift-operators
```