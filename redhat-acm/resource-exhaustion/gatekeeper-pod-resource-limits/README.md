# Gatekeeper Disallow Pod Resource Limits

The policy disallows creating pods/service/deployment/deploymentconfig/replicaset (and every object that includes pod creation in any shape) that is does not have resource limits / requests associated with it.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/resource-exhaustion/pod-resource-limits) in order to function.