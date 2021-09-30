# Disallow EmptyDir Volumes

The policy disallows associating an `emptyDir` volume to containers.

`emptyDir` volumes store data on the worker nodes and may lead to resource exhaustion. Storing data on persistent volumes is recommended to avoid resource exhaustion attacks on the platform.

`This policy has been tested on openshift cluster & oc client version 4.8.11`

The required procedure to deploy the policy:

1. Deploy Gatekeeper operator
3. Deploy the template & constraint yamls that define the policy
4. Deploy test Deployment object to make sure the policy works as expected

`You should be logged in as cluster-admin privilaged user`

## Deploy the template & constraint yamls that define the policy

```bash
oc apply -f template.yaml -n openshift-gatekeeper-system
oc apply -f constraint.yaml -n openshift-gatekeeper-system

oc get constrainttemplate/k8sdisallowemptydir -n openshift-gatekeeper-system
oc describe k8sdisallowemptydir -n openshift-gatekeeper-system
```

## Deploy Deployment object to make sure the policy works as expected
```bash

# Should be rejected
# Deployment has an emptyDir volume associated with it
oc apply -f ./test/deployment.yaml

```
