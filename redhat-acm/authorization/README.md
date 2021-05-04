# Red Hat Advanced Cluster Management for Kubernetes Authrorization Policies

This directory includes a list of Red Hat Advanced Cluster Management for Kubernetes policies that focus on authorization in OpenShift.

## [gatekeeper-disalllow-privileged-scc-usage.yaml](./gatekeeper-disalllow-privileged-scc-usage.yaml) 

The policy disallows adding users, service accounts and groups to the `privileged` SecurityContextConstraints object. In order to assign a specific user to the `privileged` SecurityContextConstraints, make sure to exclude the entity in the policy's `constraints.gatekeeper.sh/v1beta1` object definition.

To add the `foo` service account in the `bar` namespace to the `privileged` SecurityContextConstraints, modify the [gatekeeper-disalllow-privileged-scc-usage.yaml](./gatekeeper-disalllow-privileged-scc-usage.yaml) file -

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

After applying the updated `gatekeeper-disalllow-privileged-scc-usage.yaml` object, the service account could be added to the `privileged` SecurityContextConstraints by editing the object -
```
<managed cluster> $ oc edit scc privileged

...
users:
...
- system:serviceaccount:bar:foo
...
```