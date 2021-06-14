# Gatekeeper Prevenet Default Serviceaccount Usage

The policy disallows creating pods/service/deployment/deploymentconfig/replicaset (and every object that includes pod creation in any shape) that is not associated with a dedicated service account.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authorization/prevent-default-serviceaccount-usage) in order to function.
