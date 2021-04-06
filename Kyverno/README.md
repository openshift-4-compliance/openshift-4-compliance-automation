# Kyverno Policies
Policies in this folder are based on [Kyverno - Kubernetes Native Policy Management](https://kyverno.io/docs/introduction/). The policies in this folder are based on security controls from the [OCP 4.x Platform &Infrastructure Security Best Practices](https://github.com/rhilconsultants/openshift/blob/master/featureReference/Red%20Hat%20Openshift%204.x%20Security%20Best%20Practices%20-%20Public%20Edition%20-%20Final%20v2%20(2).pdf) compliance document. Each security control will be represented by its own directory.

The solution presented in this directory is designed to provide compliance for  [Red Hat OpenShift](https://www.openshift.com/) clusters. Note that some policies can be applied _out of the box_, while other policies may require changes according to organization regulations.

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

### Authorization
Policy  | Description | Prerequisites
------- | ----------- | -------------
[protect-default-scc](./authorization/protect-default-scc.yaml) | Ensures that default scc are not being modified or deleted |

### ETCD Security
Policy  | Description | Prerequisites
------- | ----------- | -------------

### Infrastructure General
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  | 

### Monitoring and Observability
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  |

### Networking
Policy  | Description | Prerequisites
------- | ----------- | -------------

### Resource Exhaustion
Policy  | Description | Prerequisites
------- | ----------- | -------------

### Storage
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  |

### Trusted Image Sources
Policy  | Description | Prerequisites
------- | ----------- | -------------

## Applying Policies
The policies can be created by applying the custom resources defined in the `template.yaml` and `contraint.yaml` files to an OpenShift cluster. The files are provided in each policy directory under specific security control.

For example, applying 'kubeadmin' temporary user removal validation policy

```
$ oc apply -f https://raw.githubusercontent.com/openshift-4-compliance/openshift-4-compliance-automation/master/open-policy-agent/authentication-user-management/delete-kubeadmin/template.yaml
$ oc apply -f https://raw.githubusercontent.com/openshift-4-compliance/openshift-4-compliance-automation/master/open-policy-agent/authentication-user-management/delete-kubeadmin/constraint.yaml
```
