# ServiceAccount Automount Token Prevention

The policy disallows creating serviceAccounts that does not include the following key:value pair in its manifest: `"automountServiceAccountToken": false`

This policy does not monitor the three default serviceaccounts that are getting generated in new Openshift projects - default, builder, deployer

It is useful (security-wise) so that by default services' pods won't be able to mount the token of the serviceAccount which deployed them if they aren't meant to. In case that a serviceAccount token is required it can be excluded.

`This policy has been tested on openshift cluster & oc client version 4.6.9`

The required procedure to deploy the policy:

1. Add the subscription for the gatekeeper operator
2. Generate the relevant gatekeeper object CR
3. Deploy the template & constraint yamls that define the policy
* Note that any openshift-* & kubernetes-* default namespaces are excluded
4. Deploy the test serviceAccount objects to make sure the policy works as expected

`You should be logged in as cluster-admin privilaged user`


## Deploy the template & constraint yamls that define the policy

```bash
oc apply -f template.yaml -n openshift-gatekeeper-system
oc apply -f constraint.yaml -n openshift-gatekeeper-system

oc describe constrainttemplate/k8sserviceaccountautomounttokenprevention
oc describe k8sserviceaccountautomounttokenprevention
```

## Deploy test pods & deployment object to make sure the policy works as expected
```bash

oc new-project test

# expect failure
# No automountServiceAccountToken <=> not approved 
oc apply -f ./test/service_account_should_not_work_basic.yaml -n test

# automountServiceAccountToken == false <=> approved
# automountServiceAccountToken != false <=> not approved 
oc apply -f ./test/service_account_should_not_work_advanced.yaml -n test


# expect success
oc apply -f ./test/service_account_should_work.yaml -n test

## Test namespace excluding ##

# expect success
oc apply -f ./test/service_account_should_not_work_basic.yaml -n openshift-operators
oc delete -f ./test/service_account_should_not_work_basic.yaml -n openshift-operators
oc apply -f ./test/service_account_should_not_work_advanced.yaml -n openshift-operators
oc delete -f ./test/service_account_should_not_work_advanced.yaml -n openshift-operators
```
