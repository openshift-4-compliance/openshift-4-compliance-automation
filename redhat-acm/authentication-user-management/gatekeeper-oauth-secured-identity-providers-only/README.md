# Gatekeeper OAuth Secured Identity Providers Only

The policy ensures that only secured identityProviders (based on a dynamic,customable list from the constraint obejct) are allowed on the cluster. 
The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authorization/disallow-privileged-scc-usage) in order to function.