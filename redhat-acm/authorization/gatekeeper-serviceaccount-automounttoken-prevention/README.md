# Gatekeeper ServiceAccount Automount Token Prevention Usage

The policy disallows creating serviceAccounts that does not include the following key:value pair in its manifest: "automountServiceAccountToken": false

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authorization/disallow-privileged-scc-usage) in order to function.