# Usage guide

Prior to applying the policy, make sure the Gatekeeper Operator is installed on the managed cluster

```bash
oc apply -f https://raw.githubusercontent.com/openshift-4-compliance/openshift-4-compliance-automation/master/redhat-acm/infrastructure-general/gatekeeper-operator-policy.yaml
```

```bash
oc create -f unique-serviceaccount-per-pod-policy.yaml

oc create -f serviceaccount-automount-token-prevention-policy.yaml
```