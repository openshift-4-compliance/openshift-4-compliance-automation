# Red Hat Advanced Cluster Management Policies
Policies in this folder are based on [Red Hat Advanced Cluster Management for Kubernetes](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.2/). The policies in this folder are based on security controls from the [OCP 4.x Platform &Infrastructure Security Best Practices](https://github.com/rhilconsultants/openshift/blob/master/featureReference/Red%20Hat%20Openshift%204.x%20Security%20Best%20Practices%20-%20Public%20Edition%20-%20Final%20v2%20(2).pdf) compliance document. Each security control will be represented by its own directory.

The solution presented in this directory is designed to provide compliance accross a multi cluster [Red Hat OpenShift](https://www.openshift.com/) environment. Note that some policies can be applied _out of the box_, while other policies may require changes according to organization regulations.

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
[kubeadmin-policy](./authentication-user-management/kubeadmin-policy.yaml) | Validates the removal of the kubeadmin temporary user |
[group-policy](./authentication-user-management/group-policy.yaml) | Ensures that a group is created with the defined users in it |
[gatekeeper-shorten-tokens](./authentication-user-management/gatekeeper-shorten-tokens.yaml) | Ensure that authentication tokens have a restricted lifespan | 

### Authorization
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallowed-role-policy](./authorization/disallowed-role-policy.yaml) | Ensures that the defined role pattern does not exist in the cluster |
[role-policy](./authorization/role-policy.yaml) | Ensures that a role exists in the cluster |
[role-binding-policy](./authorization/role-binding-policy.yaml) | Ensures that a role is bound to a user / group |

### ETCD Security
Policy  | Description | Prerequisites
------- | ----------- | -------------
[etcdencryption-policy](./etcd-security/etcdencryption-policy.yaml) | Ensures that the etcd database is encrypted |
[etcd-backup-policy](./etcd-security/etcdbackup-policy.yaml) | Backup the etcd data on a weekly basis into a PersistentVolume and rotate the backups to avoid over consumption |

### Infrastructure General
Policy  | Description | Prerequisites
------- | ----------- | -------------
[gatekeeper-operator-policy](./infrastructure-general/gatekeeper-operator-policy.yaml) | Ensures that the Gatekeeper operator is running |

### Monitoring and Observability
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  |

### Networking
Policy  | Description | Prerequisites
------- | ----------- | -------------
[allow-port-from-namespace-networkpolicy-policy](./networking/deny-all-networkpolicy-policy.yaml) | Ensures that all traffic to a namespace is denied by default |
[allow-port-from-namespace-networkpolicy-policy](./networking/allow-port-from-namespace-networkpolicy-policy.yaml) | Ensures that a custom NetworkPolicy is available in a namespace |
[gatekeeper-allow-httpsonly](./networking/gatekeeper-allow-httpsonly.yaml) | Ensures that there are *no* http routes | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[gatekeeper-disallow-external-ip-services](./networking/gatekeeper-disallow-external-ip-services.yaml) | Ensures that there are no external ip services configured | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[gatekeeper-disallow-nodeport](./networking/gatekeeper-disallow-nodeport.yaml) | Ensures that there are no node port services configured | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed

### Resource Exhaustion
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallow-self-provisioner-policy](./redhat-acm/disallow-self-provisioner-policy.yaml) | Ensures that users are not able to provision new namespaces by validating the removal of the `self-provisioners` ClusterRole |
[limitrange-policy](./resource-exhaustion/limitrange-policy.yaml) | Ensures that a Limitrange resource is present in a namespace |

### Storage
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  |

### Trusted Image Sources
Policy  | Description | Prerequisites
------- | ----------- | -------------
[gatekeeper-disallow-image-tags](./networking/gatekeeper-disallow-image-tags.yaml) | Ensures that images do not contain a pre-defined tag (by default, the policy disables the `latest` tag) | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed

## Applying Policies
The policies can be applied to the Advanced Cluster Management hub cluster. By default all policies take effect on all managed clusters with the `environment=dev` label. To apply a policy from the collection, run the next command -

```
<hub cluster> $ oc apply -f https://raw.githubusercontent.com/michaelkotelnikov/openshift-4-compliance-automation/master/redhat-acm/authorization/disallowed-role-policy.yml
```

To deploy policies using the GitOps approach, please refer to the next article - [Contributing and deploying community policies with Red Hat Advanced Cluster Management and GitOps](https://www.openshift.com/blog/contributing-and-deploying-community-policies-with-red-hat-advanced-cluster-management-and-gitops).