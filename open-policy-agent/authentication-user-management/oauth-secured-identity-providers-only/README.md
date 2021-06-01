# OAuth Secured IdentityProviders Only

The policy ensures that only secured identityProviders (based on a dynamic, customizable list from the [constraint obejct](./constraint.yaml)) are allowed in the cluster. 

The recommended identityProvider is OpenID Connect, and thus, the list in the [constraint obejct](./constraint.yaml) currently contains only this identityProvider. The list can be expended if the organization requires other identity providers.

`This policy has been tested on openshift cluster & oc client version 4.7.0`

The required procedure to deploy the policy:

1. Add the subscription for the gatekeeper operator.
2. Generate the relevant gatekeeper object CR.
3. Deploy the [template](./template.yaml) & [constraint](./constraint.yaml) yamls that define the policy.

`You should be logged in as cluster-admin privilaged user`

## Deploy the template & constraint yamls that define the policy

```bash
oc apply -f template.yaml -n openshift-gatekeeper-system
oc apply -f constraint.yaml -n openshift-gatekeeper-system
```
