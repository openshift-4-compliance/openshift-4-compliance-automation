# Namespace Regulation for Pods

The policy disallows pods from running on specific namespaces. The dissalowed namespaces should be provided as a parameter.

Running workload on undesiered namespace can be a potential security risk as they might run without being monitored and regulated.

`This policy has been tested on openshift cluster & oc client version 4.9.0`

The required procedure to deploy the policy:

1. Deploy Gatekeeper operator
3. Deploy the template & constraint yamls that define the policy
4. Deploy Pods on undesiered namespace to make sure the policy prevents it

## Deploy the template & constraint yamls that define the policy

```bash
oc apply -f template.yaml -n openshift-gatekeeper-system
oc apply -f constraint.yaml -n openshift-gatekeeper-system

oc get constrainttemplate/k8sdisallowanonymous -n openshift-gatekeeper-system
oc describe k8sdisallowanonymous -n openshift-gatekeeper-system
```

## Deploy ClusterRoleBinding object to make sure the policy works as expected
```bash

# Should be rejected
# ClusterRoleBinding has system:unauthenticated in its subjects
oc apply -f ./test/disallowed-clusterrolebinding.yaml

```
