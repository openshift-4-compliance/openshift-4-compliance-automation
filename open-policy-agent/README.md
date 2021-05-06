# OPA Gatekeeper Policies
Policies in this folder are based on [OPA Gatekeeper for Kubernetes](https://www.openpolicyagent.org/docs/latest/kubernetes-introduction/). The policies in this folder are based on security controls from the [OCP 4.x Platform &Infrastructure Security Best Practices](https://github.com/rhilconsultants/openshift/blob/master/featureReference/Red%20Hat%20Openshift%204.x%20Security%20Best%20Practices%20-%20Public%20Edition%20-%20Final%20v2%20(2).pdf) compliance document. Each security control will be represented by its own directory.

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
[delete-kubeadmin](./authentication-user-management/delete-kubeadmin) | Validates the removal of the kubeadmin temporary user |
[shorten-tokens](./authentication-user-management/authentication-user-management) | Validate that tokens are shorter than the defined lifespan period |

### Authorization
Policy  | Description | Prerequisites
------- | ----------- | -------------
[no-priv-scc](./authorization/no-priv-scc) | Ensures that privilged scc is not being used by unlisted Services |

### ETCD Security
Policy  | Description | Prerequisites
------- | ----------- | -------------
[verify-etcd-encryption](./etcd-security/verify-etcd-encryption) | Ensures that the etcd is encrypted properly |

### ServiceAccount Management
Policy  | Description | Prerequisites
------- | ----------- | -------------
[unique-serviceaccount-per-pod](./service-account-mgmt/unique_serviceaccount_per_pod) | Ensures that every pod/deployment etc. has a unique serviceaccount attached to it |
[serviceaccount-automount-token-prevention](./service-account-mgmt/serviceaccount_automounttoken_prevention) | Ensures that serviceAccounts' tokens are unmountable by default  |


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
[block-nodeport-services](./networking/block-nodeport-services) | Ensures that there are no node port services configured |
[external-ips](./networking/external-ips) | Ensures that there are no external ip services configured |
[httpsonly](./networking/httpsonly) | Ensures that there are *no* http routes | 


### Resource Exhaustion
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallow-selfprovisioners](./resource-exhaustion/disallow-self-provisioner/) | Ensures that there are no users assigned to the self-provisioner ClusterRole |


### Storage
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  |

### Trusted Image Sources
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallowedtags](./trusted-image-sources/disallowedtags) | Ensures that images do not contain a pre-defined tag (by default, the policy disables the `latest` tag) | 

## Applying Policies
The policies can be created by applying the custom resources defined in the `template.yaml` and `contraint.yaml` files to an OpenShift cluster. The files are provided in each policy directory under specific security control.

For example, applying 'kubeadmin' temporary user removal validation policy

```
$ oc apply -f https://raw.githubusercontent.com/openshift-4-compliance/openshift-4-compliance-automation/master/open-policy-agent/authentication-user-management/delete-kubeadmin/template.yaml
$ oc apply -f https://raw.githubusercontent.com/openshift-4-compliance/openshift-4-compliance-automation/master/open-policy-agent/authentication-user-management/delete-kubeadmin/constraint.yaml
```
