# Prevent Default Serviceaccount

The policy pop an alert (dry-run) once a cluster-admin privilages has been granted to unapproved subjects (users, groups, serviceaccounts).

Cluster-Admin is the most powerful ClusterRole that Openshift has for its users; Once it has been granted to a subject - this subject can perform any operation on the entire cluster; The best practice for managing it appropriatly is to raise an alert for any unauthorized entity that it has been granted to in the environment.

`This policy has been tested on openshift cluster & oc client version 4.7.0`

The required procedure to deploy the policy:

1. Add the subscription for the gatekeeper operator
2. Generate the relevant gatekeeper object CR
3. Deploy the template & constraint yamls that define the policy
* Note that any openshift-* & kubernetes-* default namespaces are excluded
4. Run the test commands to make sure the policy works as expected

`You should be logged in as cluster-admin privilaged user`

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


# Wait for a few minutes
oc describe k8sdisallowclusteradmin.constraints.gatekeeper.sh/no-cluster-admin
```

![image](https://user-images.githubusercontent.com/60185557/125775671-566238fc-ecd7-4d4b-9cce-c6d56214e5dd.png)

`Note!` In your environment you should see 3 total violations; In my environment I had another user that caused an extra violation to pop up.


`Note!` The policy does not specificy the exact namespace in which the subject is configured (relevant for serviceaccount only. Users & Groups are cluster-wide entities) - but it is written within the ClusterRoleBinding that the violation mention. In order to get the relevant information run the following command:

```bash
oc describe clusterrolebinding <name-of-the-binding>
```

Example (based on the output from the previous screenshot)

![image](https://user-images.githubusercontent.com/60185557/125776066-789ce1bd-634c-45ec-b635-1a5e193f23f5.png)
