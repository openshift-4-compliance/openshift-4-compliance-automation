# Disallow EmptyDir Volumes

The policy disallows creating pods/service/deployment/deploymentconfig/replicaset (and every object that includes pod creation in any shape) that associates with an EmptyDir volume.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/storage/disallow-emptydir) in order to function.