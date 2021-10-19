# Prevent Default ServiceAccount

The policy disallows creating pods/service/deployment/deploymentconfig/replicaset (and every object that includes pod creation in any shape) that is not associated with a dedicated service account.

Generating a dedicated service account per micro service is a common security practice for developers. Creating a service account per micro service provides more control over the pod's permissions and audit logs, without affecting other micro services.

`This policy has been tested on openshift cluster & oc client version 4.6.9`

The required procedure to deploy the policy:

1. Add the subscription for the gatekeeper operator
2. Generate the relevant gatekeeper object CR
3. Deploy the template & constraint yamls that define the policy
* Note that any openshift-* & kubernetes-* default namespaces are excluded
4. Deploy test pods & deployment object to make sure the policy works as expected

`You should be logged in as cluster-admin privileged user`

## Deploy the template & constraint yamls that define the policy

```bash
oc apply -f template.yaml -n openshift-gatekeeper-system
oc apply -f constraint.yaml -n openshift-gatekeeper-system

oc describe ConstraintTemplate/k8spoduniqueserviceaccountvalidation -n openshift-gatekeeper-system
oc describe K8sPodUniqueServiceAccountValidation -n openshift-gatekeeper-system
```

## Deploy test pods & deployment object to make sure the policy works as expected
```bash
oc new-project test
oc create serviceaccount test -n test
oc create serviceaccount not-test -n test

# Should be rejected
# No "serviceAccountName"
oc apply -f ./test/not_approved_pod_1.yaml -n test

# Should work fine
# No spec.serviceAccountName == "some-sa"
oc apply -f ./test/approved_pod.yaml -n test

# Examine violation
oc describe k8spoduniqueserviceaccountvalidation.constraints.gatekeeper.sh/pod-unique-serviceaccount-validation -n openshift-gatekeeper-system

# Should work because of namespace excluding
# No "serviceAccountName"
oc apply -f ./test/not_approved_pod_1.yaml -n openshift-operators

# Should not work as well
oc apply -f ./test/test_deployment_not_approved.yaml -n test

# Verify option 1
oc get events | grep -i hello-openshift

# Verify option 2
oc get events -n openshift-gatekeeper-system | grep -i hello-openshift
```
