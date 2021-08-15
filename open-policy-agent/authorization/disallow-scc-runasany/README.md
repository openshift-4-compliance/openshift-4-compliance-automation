# Disallow SCC RunAsAny

The policy disallows assigning a value other than `MustRunAsRange` to the `RunAsUser` field in SecurityContextConstraints objects. In order to exclude specific SecurityContextConstraints objects from being monitored by Gatekeeper, make sure to add them in the policy's [constraint.yaml](./constraint.yaml) object.

To add the `foo` SecurityContextConstraint to the list of excluded SecurityContextConstraints objects, modify the [constraint.yaml](./constraint.yaml) file -

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sNoPrivScc
metadata:
  name: no-privileged-sccs
...
  parameters:
    allowedSCCs:
    ...
      - "foo"
    ...
```

After applying the updated `constraint.yaml` object, the `MustRunAsRange` value could be added to the `RunAsUser` attribute in `foo` SecurityContextConstraint object. Editing the `foo` SecurityContextConstraint -
```
$ oc edit scc foo

...
runAsUser:
  type: RunAsAny
...
```