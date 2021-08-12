# Gatekeeper Prevent ServiceAccount Automount Token Usage

The policy disallows creating serviceAccounts that does not include the following key:value pair in its manifest: `automountServiceAccountToken: false`

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authorization/serviceaccount-automounttoken-prevention) in order to function.