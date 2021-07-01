# Gatekeeper Disallow Host Namespaces

The policy disallows creating pods/service/deployment/deploymentconfig/replicaset (and every object that includes pod creation in any shape) that has the `hostPID` or `hostIPC` specs associated with it.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authorization/disallow-host-namespaces) in order to function.