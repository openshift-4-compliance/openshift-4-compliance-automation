# Gatekeeper Prevenet Default Serviceaccount Usage

The policy disallows assigning a value of `RunAsAny` to the `RunAsUser` field in SecurityContextConstraints objects. In order to exclude specific SecurityContextConstraints objects from being monitored by Gatekeeper, make sure to add them in the policy's constraint definition.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authorization/disallow-scc-runasany) in order to function.
