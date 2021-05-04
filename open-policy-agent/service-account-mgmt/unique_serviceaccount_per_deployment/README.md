# [unique_serviceaccount_per_deployment_or_pod](./disallow-privileged-scc-usage) 

This policy disallows adding users, service accounts and groups to the `privileged` SecurityContextConstraints object. In order to assign a specific user to the `privileged` SecurityContextConstraints, make sure to exclude the entity in the policy's [constraint.yaml](./disallow-privileged-scc-usage/constraint.yaml) object.

To add the `foo` service account in the `bar` namespace to the `privileged` SecurityContextConstraints, modify the [constraint.yaml](./disallow-privileged-scc-usage/constraint.yaml) file -

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sNoPrivScc
metadata:
  name: no-privileged-sccs
...
  parameters:
    allowedUsers:
    ...
      - "system:serviceaccount:bar:foo"
    ...
```

After applying the updated `constraint.yaml` object, the service account could be added to the `privileged` SecurityContextConstraints by editing the object -
```
$ oc edit scc privileged
...
users:
...
- system:serviceaccount:bar:foo
...
```