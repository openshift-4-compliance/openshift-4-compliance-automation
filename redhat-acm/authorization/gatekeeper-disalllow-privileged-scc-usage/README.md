# Gatekeeper Disalllow Privileged SCC Usage

The policy disallows adding users, service accounts and groups to the `privileged` SecurityContextConstraints object. The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authorization/disallow-privileged-scc-usage) in order to function.