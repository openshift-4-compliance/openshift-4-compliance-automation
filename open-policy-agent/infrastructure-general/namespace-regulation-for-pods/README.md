# Namespace Regulation for Pods

The policy disallows pods from running on specific namespaces. The disallowed namespaces must be provided as a parameter.

Running workload on undesired namespace can be a potential security risk as they might run without being monitored and regulated.
Placing objects in the default namespace, for example, makes application of RBAC and other controls more difficult.

`This policy has been tested on openshift cluster & oc client version 4.9.0`

The required procedure to deploy the policy:

1. Deploy Gatekeeper operator
3. Deploy the template & constraint yamls that define the policy
4. Deploy Pods on undesired namespace to make sure the policy prevents it

## Deploy the template & constraint yamls that define the policy

```bash
oc apply -f template.yaml -n openshift-gatekeeper-system
oc apply -f constraint.yaml -n openshift-gatekeeper-system

oc get constrainttemplate/k8snamespaceregulation -n openshift-gatekeeper-system
oc describe k8snamespaceregulation -n openshift-gatekeeper-system
```

## Deploy Pod object to make sure the policy works as expected
```bash

# Create Pod in default namespace
# Should be rejected
oc apply -f ./test/default-pod.yaml

```
