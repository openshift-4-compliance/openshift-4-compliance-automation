# Disallow Anonymous Authentication

The policy disallows associating the `system:anonymous` User and `system:unauthenticated` Group with any ClusterRole / Role in the cluster.

Associating unauthenticated users with ClusterRoles / Roles in the cluster may open a doorway for potential attacks. The unauthenticated users are not provided via an authorized identity provider, thereby, not secure.

`This policy has been tested on openshift cluster & oc client version 4.8.4`

The required procedure to deploy the policy:

1. Deploy Gatekeeper operator
3. Deploy the template & constraint yamls that define the policy
4. Deploy test ClusterRoleBinding object to make sure the policy works as expected

`You should be logged in as cluster-admin privilaged user`

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
