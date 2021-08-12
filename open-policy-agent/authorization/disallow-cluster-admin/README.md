# Prevent Default Serviceaccount

The policy initiates an alert once a cluster-admin privilages have been granted to unapproved subjects (users, groups, serviceaccounts).

cluster-admin is the most powerful ClusterRole that Openshift has for its users; Once it has been granted to a subject - this subject can perform any operation on the entire cluster; The best practice for managing it appropriatly is to raise an alert for any unauthorized entity that it has been granted to in the environment.

`This policy has been tested on openshift cluster & oc client version 4.7.0`

The required procedure to deploy the policy:

1. Add the subscription for the gatekeeper operator
2. Generate the relevant gatekeeper object CR
3. Deploy the template & constraint yamls that define the policy
* Note that any openshift-* & kubernetes-* default namespaces are excluded
4. Run the test commands to make sure the policy works as expected

`cluster-admin privilage is required to run the next commands`

## Deploy the template & constraint yamls that define the policy

```bash
oc apply -f template.yaml
oc apply -f constraint.yaml

oc describe k8sdisallowclusteradmin.constraints.gatekeeper.sh/no-cluster-admin
oc describe constrainttemplate.templates.gatekeeper.sh/k8sdisallowclusteradmin
```

## Run the following commands to make sure the policy works as expected
```bash
oc new-project test
oc create serviceaccount test-sa -n test
oc create user test-user
oc adm groups new test-group

oc adm policy add-cluster-role-to-user cluster-admin test-user
oc adm policy add-cluster-role-to-user cluster-admin -z test-sa -n test
oc adm policy add-cluster-role-to-group cluster-admin test-group

oc describe k8sdisallowclusteradmin.constraints.gatekeeper.sh/no-cluster-admin
```

`Note!` 3 violations should be initiated.


`Note!` The policy does not specify the exact namespace in which the subject is configured (relevant for serviceaccount only; Users & Groups are cluster-wide entities). The information regarding the ServiceAccount's namespace is written within the violating ClusterRoleBinding. In order to get the relevant information run the following command:

```bash
oc describe clusterrolebinding <name-of-the-binding>
```
