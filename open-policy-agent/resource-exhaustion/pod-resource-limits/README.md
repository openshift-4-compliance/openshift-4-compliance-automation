# Pod Resource Limits

The policy disallows creating pods/service/deployment/deploymentconfig/replicaset (and every object that includes pod creation in any shape) that is does not have resource limits / requests associated with it.

Associating limits per container in a deployment is a common security practice for developers. It provides control over container resource consumption, and prevents resource exhaustion attacks.

`This policy has been tested on openshift cluster & oc client version 4.6.9`

The required procedure to deploy the policy:

1. Add the subscription for the gatekeeper operator
2. Generate the relevant gatekeeper object CR
3. Deploy the template & constraint yamls that define the policy
* Note that any openshift-* & kubernetes-* default namespaces are excluded
4. Deploy test pods & deployment object to make sure the policy works as expected

`You should be logged in as cluster-admin privilaged user`

## Deploy the template & constraint yamls that define the policy

```bash
oc apply -f template.yaml -n openshift-gatekeeper-system
oc apply -f constraint.yaml -n openshift-gatekeeper-system

oc describe ConstraintTemplate/k8spodresourcelimit -n openshift-gatekeeper-system
oc describe K8sPodResourceLimit -n openshift-gatekeeper-system
```

## Deploy test pods & deployment object to make sure the policy works as expected
```bash
oc new-project test

# Should be rejected
# No resource requests / limits defined
oc apply -f ./test/not_approved_deployment.yaml -n test

# Examin violation
oc get replicaset

oc describe replicaset/<replicaset-name>

oc get k8spodresourcelimit.constraints.gatekeeper.sh/no-resource-limit-pods -o yaml

# Should work fine
# Resource requests and limits defined
oc apply -f ./test/approved_deployment.yaml -n test
```