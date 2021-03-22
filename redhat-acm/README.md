# Red Hat Advanced Cluster Management Policies
Policies in this folder are based on Red Hat Advanced Cluster Management for Kubernetes. The policies in this folder are based on security controls from the [OCP 4.x Platform &Infrastructure Security Best Practices](https://github.com/rhilconsultants/openshift/blob/master/featureReference/Red%20Hat%20Openshift%204.x%20Security%20Best%20Practices%20-%20Public%20Edition%20-%20Final%20v2%20(2).pdf) compliance document. Each security control will be represented by its own directory.

## Security control catalog
- [authentication-user-management](./authentication-user-management)
- [authorization](./authorization)
- [etcd-security](./etcd-security)
- [infrastructure-general](./infrastructure-general)
- [monitoring-observability](./monitoring-observability)
- [networking](./networking)
- [resource-exhaustion](./resource-exhaustion)
- [storage](./storage)
- [trusted-image-sources](./trusted-image-sources)


### Authentication and User Management
Policy  | Description | Prerequisites
------- | ----------- | -------------
[kubeadmin-policy](./authentication-user-management/kubeadmin-policy.yml) | Validates the removal of the kubeadmin temporary user |

### Authorization
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallowed-role-policy](./authorization/disallowed-role-policy.yml) | Ensures that the defined role pattern does not exist in the cluster |
[role-policy](./authorization/role-policy.yml) | Ensures that a role exists in the cluster |
[role-binding-policy](./authorization/role-binding-policy.yml) | Ensures that a role is bound to a user / group |
