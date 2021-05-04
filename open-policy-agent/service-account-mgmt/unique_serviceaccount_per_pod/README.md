# Policy Explainer & Testing - Unique Serviceaccount Per Pod 

The policy disallows creating pods/service/deployment/deploymentconfig/replicaset (and every object that includes pod creation in any shape) that does not include a dedicated service account with name equals to the name of the pod itself.

It is useful (security-wise) so that developers will get use to generate dedicated serviceAccount per micro-service, so it will be easier to audit its logs and grant elevated permissions to serviceaccounts that really requires it, without effecting multiple services at once.

The required procedure to deploy the policy:

1. Add the subscription for the gatekeeper operator
2. Generate the relevant gatekeeper object CR
3. Deploy the template & constraint yamls that define the policy
* Note that any openshift-* & kubernetes-* default namespaces are excluded
4. Deploy test pods & deployment object to make sure the policy works as expected

`You should be logged in as cluster-admin privilaged user`

## Add the subscription for the gatekeeper operator

```bash
cat > subscription.yaml << EOF
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: gatekeeper-operator-product
  namespace: openshift-operators
spec:
  channel: stable
  installPlanApproval: Automatic
  name: gatekeeper-operator-product
  source: redhat-operators
  sourceNamespace: openshift-marketplace
EOF

oc create -f subscription.yaml
oc get subscriptions.operators.coreos.com -A | grep -i gatekeeper
```


## Generate the relevant gatekeeper object CR

```bash
cat > gatekeeper_cr.yaml << EOF
apiVersion: operator.gatekeeper.sh/v1alpha1
kind: Gatekeeper
metadata:
  name: gatekeeper
spec:
  audit:
    logLevel: INFO
    replicas: 1
  image:
    image: 'registry.redhat.io/rhacm2/gatekeeper-rhel8:v3.3.0'
  validatingWebhook: Enabled
  mutatingWebhook: Disabled
  webhook:
    emitAdmissionEvents: Enabled
    logLevel: INFO
    replicas: 2
EOF

oc create -f gatekeeper_cr.yaml
oc get pods -n openshift-gatekeeper-system

```

## Deploy the template & constraint yamls that define the policy

```bash
cat > constraint.yaml << EOF
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sPodUniqueServiceAccountValidation
metadata:
  name: pod-unique-serviceaccount-validation
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
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
  name: k8spoduniqueserviceaccountvalidation
  annotations:
    description: Checks wheter the pod has a unique serviceAccount, correlated with the pod's name
spec:
  crd:
    spec:
      names:
        kind: K8sPodUniqueServiceAccountValidation
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package K8sPodUniqueServiceAccountValidation

        missing(obj, field) = true {
          not obj[field]
        }
        
        missing(obj, field) = true {
          obj[field] == ""
        }

        violation[{"msg": msg}] {
          pod := input.review.object

          missing(pod.spec, "serviceAccountName")/k8spoduniqueserviceaccountvalidation one.", [input.review.object.metadata.name])
        }
EOF

oc create -f template.yaml -n openshift-gatekeeper-system
oc create -f constraint.yaml -n openshift-gatekeeper-system

oc describe ConstraintTemplate/k8spoduniqueserviceaccountvalidation -n openshift-gatekeeper-system
oc describe K8sPodUniqueServiceAccountValidation -n openshift-gatekeeper-system
```

## Deploy test pods & deployment object to make sure the policy works as expected
```bash
# No spec.serviceAccountName == metadata.name == "test"
cat > approved_pod.yaml << EOF
apiVersion: v1
kind: Pod
metadata:
  name: test
spec:
  serviceAccountName: test
  containers:
   - name: web
     image: nginx
     ports:
     - name: web
       containerPort: 80
       protocol: TCP
EOF

# No "serviceAccountName"
cat > not_approved_pod_1.yaml << EOF
apiVersion: v1
kind: Pod
metadata:
  name: test
spec:
  containers:
   - name: web
     image: nginx
     ports:
     - name: web
       containerPort: 80
       protocol: TCP
EOF

# No spec.serviceAccountName ("not-test") != metadata.name ("test") 
cat > not_approved_pod_2.yaml << EOF
apiVersion: v1
kind: Pod
metadata:
  name: test
spec:
  serviceAccountName: not-test
  containers:
   - name: web
     image: nginx
     ports:
     - name: web
       containerPort: 80
       protocol: TCP
EOF

# No "serviceAccountName"
cat > test_deployment_not_approved.yaml << EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-openshift
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-openshift
  template:
    metadata:
      labels:
        app: hello-openshift
    spec:
      containers:
      - name: hello-openshift
        image: openshift/hello-openshift:latest
        ports:
        - containerPort: 80
EOF
```

```bash
oc new-project test
oc create serviceaccount test -n test
oc create serviceaccount not-test -n test

# Should be rejected
oc create -f not_approved_pod_1.yaml -n test

# Should be rejected
oc create -f not_approved_pod_2.yaml -n test

# Should work fine
oc create -f approved_pod.yaml -n test

# Examin violation
oc describe k8spoduniqueserviceaccountvalidation.constraints.gatekeeper.sh/pod-unique-serviceaccount-validation -n openshift-gatekeeper-system

# Should work because of namespace excluding
oc create -f not_approved_pod_1.yaml -n openshift-operators

# Should not work as well
oc create -f test_deployment_not_approved.yaml -n test

# Verify option 1
oc get events | grep -i hello-openshift

# Verify option 2
oc get events -n openshift-gatekeeper-system | grep -i hello-openshift
```