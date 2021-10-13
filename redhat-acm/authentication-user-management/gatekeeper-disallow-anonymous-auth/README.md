# Disallow Anonymous Authentication

The policy disallows associating the `system:anonymous` User and `system:unauthenticated` Group with any ClusterRole / Role in the cluster.

Associating unauthenticated users with ClusterRoles / Roles in the cluster may open a doorway for potential attacks. The unauthenticated users are not provided via an authorized identity provider, thereby, not secure.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/authentication-user-management/disallow-anonymous-users/) in order to function.
