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
No policies yet       |  | 

### Authorization
Policy  | Description | Prerequisites
------- | ----------- | -------------
[protect-default-scc](./authorization/protect-default-scc.yaml) | Ensures that default scc are not being modified or deleted |
[disallow-host-ipc-scc](./authorization/disallow-host-ipc-scc.yaml) | Ensures that created scc are not allowing access to the host PID and IPC namespaces |

### ETCD Security
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  | 

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
[httpsonly](./networking/httpsonly.yaml) | Ensures that there are *no* http routes | 

### Resource Exhaustion
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  | 

### Storage
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  |

### Trusted Image Sources
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  | 

## Applying Policies
The policies can be created by applying the custom resources defined in the cluster policy 'yaml' files to an OpenShift cluster. The files are provided in each policy directory under specific security control.

For example, applying 'protect-default-scc' policy which prevents from default OpenShift scc to be modified or deleted by users

```
$ oc apply -f https://raw.githubusercontent.com/openshift-4-compliance/openshift-4-compliance-automation/master/kyverno/authentication-user-management/protect-default-scc.yaml
```
