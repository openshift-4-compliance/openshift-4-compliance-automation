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
[kubeadmin-policy](authentication-user-management/kubeadmin-policy/kubeadmin-policy.yaml) | Validates the removal of the kubeadmin temporary user |
[group-policy](authentication-user-management/group-policy/group-policy.yaml) | Ensures that a group is created with the defined users in it |
[gatekeeper-shorten-tokens](authentication-user-management/gatekeeper-shorten-tokens/gatekeeper-shorten-tokens.yaml) | Ensure that authentication tokens have a restricted lifespan | 

### Authorization
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallowed-role-policy](./authorization/disallowed-role-policy/disallowed-role-policy.yaml) | Ensures that the defined role pattern does not exist in the cluster |
[role-policy](./authorization/role-policy/role-policy.yaml) | Ensures that a role exists in the cluster |
[role-binding-policy](./authorization/role-binding-policy/role-binding-policy.yaml) | Ensures that a role is bound to a user / group |
[gatekeeper-disalllow-privileged-scc-usage](./authorization/gatekeeper-disalllow-privileged-scc-usage/gatekeeper-disalllow-privileged-scc-usage.yaml) | Ensures that privilged scc is not being used by unlisted service accounts, users and groups |
[default-scc-validation-policy](./authorization/default-scc-validation-policy/default-scc-validation-policy.yaml) | Ensures that the defaults SCC's are not modified |
[prevent-default-serviceaccount-usage](./authorization/gatekeeper-prevent-default-serviceaccount-usage/gatekeeper-prevent-default-serviceaccount-usage.yaml) | Ensures that the `default` serviceaccount is not usable by any pod | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[gatekeeper-disallow-host-namespaces](./authorization/gatekeeper-disallow-host-namespaces/gatekeeper-disallow-host-namespaces.yaml) | Ensures that `HostIPC` and `HostPID` are not set in the pod's definition | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[gatekeeper-disallow-host-network](./authorization/gatekeeper-disallow-host-namespaces/gatekeeper-disallow-host-network.yaml) | Ensures that `HostNetwork` and `HostPort` are not set in the pod's definition | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[gatekeeper-disallow-cluster-admin](./authorization/gatekeeper-disallow-cluster-admin/gatekeeper-disallow-cluster-admin.yaml) | Raise an alert once the cluster-admin ClusterRole is being granted to unapproved entity in the environment | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[serviceaccount-automounttoken-prevention](./authorization/gatekeeper-prevent-serviceaccount-automounttoken/gatekeeper-prevent-serviceaccount-automounttoken.yaml) | Ensures that serviceaccount's token is not mountable by default | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[gatekeeper-disallow-scc-runasany](./authorization/gatekeeper-disallow-scc-runasany/gatekeeper-disallow-scc-runasany.yaml) | Requires custom SCCs to not have the `RunAsAny` type for the `runAsUser` attribute. | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed

### ETCD Security
Policy  | Description | Prerequisites
------- | ----------- | -------------
[etcdencryption-policy](etcd-security/etcdencryption-policy/etcdencryption-policy.yaml) | Ensures that the etcd database is encrypted |
[etcd-backup-policy](etcd-security/etcdbackup-policy/etcdbackup-policy.yaml) | Backup the etcd data on a weekly basis into a PersistentVolume and rotate the backups to avoid over consumption |

### Infrastructure General
Policy  | Description | Prerequisites
------- | ----------- | -------------
[gatekeeper-operator-policy](infrastructure-general/gatekeeper-operator-policy/gatekeeper-operator-policy.yaml) | Ensures that the Gatekeeper operator is running |
[certificate-expiry-alert](infrastructure-general/certificate-expiry-alert/policy-ocp4-certs.yaml) | Raise an alert once a system certificate is about to expire |

### Monitoring and Observability
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  |

### Networking
Policy  | Description | Prerequisites
------- | ----------- | -------------
[deny-all-networkpolicy-policy](networking/deny-all-networkpolicy-policy/deny-all-networkpolicy-policy.yaml) | Ensures that all traffic to a namespace is denied by default |
[allow-port-from-namespace-networkpolicy-policy](networking/allow-port-from-namespace-networkpolicy-policy/allow-port-from-namespace-networkpolicy-policy.yaml) | Ensures that a custom NetworkPolicy is available in a namespace |
[gatekeeper-allow-httpsonly](networking/gatekeeper-allow-httpsonly/gatekeeper-allow-httpsonly.yaml) | Ensures that there are *no* http routes | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[gatekeeper-disallow-external-ip-services](networking/gatekeeper-disallow-external-ip-services/gatekeeper-disallow-external-ip-services.yaml) | Ensures that there are no external ip services configured | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[gatekeeper-disallow-nodeport](networking/gatekeeper-disallow-nodeport/gatekeeper-disallow-nodeport.yaml) | Ensures that there are no node port services configured | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed

### Resource Exhaustion
Policy  | Description | Prerequisites
------- | ----------- | -------------
[gatekeeper-disallow-self-provisioner-policy](resource-exhaustion/gatekeeper-disallow-self-provisioner-policy/gatekeeper-disallow-self-provisioner-policy.yaml) | Ensures that users are not able to provision new namespaces by disabling any ClusterRoleBinding that associates with the `self-provisioner` ClusterRole | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
[limitrange-policy](resource-exhaustion/limitrange-policy/limitrange-policy.yaml) | Ensures that a Limitrange resource is present in a namespace |
[resourcequota-policy](resource-exhaustion/resourcequota-policy/resourcequota-policy.yaml) | Ensures that a ResourceQuota resource is present in a namespace |
[pod-resource-limits](resource-exhaustion/gatekeeper-pod-resource-limits/gatekeeper-pod-resource-limits.yaml) | Ensures that all pods have a resource request / limit associated with them |

### Storage
Policy  | Description | Prerequisites
------- | ----------- | -------------
No policies yet       |  |

### Trusted Image Sources
Policy  | Description | Prerequisites
------- | ----------- | -------------
[gatekeeper-disallow-image-tags](trusted-image-sources/gatekeeper-disallow-image-tags/gatekeeper-disallow-image-tags.yaml) | Ensures that images do not contain a pre-defined tag (by default, the policy disables the `latest` tag) | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed

## Applying Policies
The policies can be applied to the Advanced Cluster Management hub cluster. By default all policies take effect on all managed clusters with the `environment=dev` label. To apply a policy from the collection, run the next command -

```
<hub cluster> $ oc apply -f https://raw.githubusercontent.com/michaelkotelnikov/openshift-4-compliance-automation/master/redhat-acm/authorization/disallowed-role-policy.yml
```

To deploy policies using the GitOps approach, please refer to the next article - [Contributing and deploying community policies with Red Hat Advanced Cluster Management and GitOps](https://www.openshift.com/blog/contributing-and-deploying-community-policies-with-red-hat-advanced-cluster-management-and-gitops).
