# Gatekeeper Disallow Self Provisioner

The policy disallows associating users with the `self-provisioner` ClusterRole. Any ClusterRoleBinding that associates to the `self-provisioner` ClusterRole will fail to create.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/resource-exhaustion/disallow-self-provisioner) in order to function.