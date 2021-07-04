# Gatekeeper Disallow Host Network

The policy disallows creating pods/service/deployment/deploymentconfig/replicaset (and every object that includes pod creation in any shape) that has the `hostNetwork` or `hostPort` specs associated with it.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authorization/disallow-host-network) in order to function.