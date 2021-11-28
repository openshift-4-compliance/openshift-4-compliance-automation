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
[shorten-tokens](./authentication-user-management/shorten-tokens) | Validate that tokens are shorter than the defined lifespan period |
[oauth-secured-identity-providers-only](./authentication-user-management/oauth-secured-identity-providers-only) | Ensures that only secured identityProviders are allowed in the cluster |
[disallow-anonymous-users](./authentication/disallow-anonymous-users) | Ensures there are no anonymous users associated with any ClusterRole / Role |

### Authorization
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallow-privileged-scc-usage](./authorization/disallow-privileged-scc-usage) | Ensures that privilged scc is not being used by unlisted service accounts, users and groups |
[prevent-default-serviceaccount-usage](./authorization/gatekeeper-prevent-default-serviceaccount-usage) | Ensures that the `default` serviceaccount is not usable by any pod |
[disallow-host-network](./authorization/disallow-host-network) | Ensures that `HostNetwork` and `HostPort` are not set in the pod's definition |
[disallow-host-namespaces](./authorization/disallow-host-namespaces) | Ensures that `HostIPC` and `HostPID` namespaces are not set in the pod's definition |
[disallow-cluster-admin](./authorization/disallow-cluster-admin) | Ensures that the `cluster-admin ClusterRole` is not overused in the environment |
[serviceaccount-automount-token-prevention](./authorization/serviceaccount_automounttoken_prevention) | Ensures that serviceAccounts' tokens are not mountable by default  |
[disallow-scc-runasany](./authorization/disallow-scc-runasany) | Requires custom SCCs to not have the `RunAsAny` type for the `runAsUser` attribute. |

### ETCD Security
Policy  | Description | Prerequisites
------- | ----------- | -------------
[verify-etcd-encryption](./etcd-security/verify-etcd-encryption) | Ensures that the etcd is encrypted properly |

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
[pod-resource-limits](./resource-exhaustion/pod-resource-limits/) | Ensures that all pods have a resource request / limit associated with them |

### Storage
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallow-emptydir](./storage/disallow-emptydir) | Ensures that containers are not associated with emptyDir volumes |

### Trusted Image Sources
Policy  | Description | Prerequisites
------- | ----------- | -------------
[disallowedtags](./trusted-image-sources/disallowedtags) | Ensures that images do not contain a pre-defined tag (by default, the policy disables the `latest` tag) |
[disallowed-registries](./trusted-image-sources/disallowedtagsdisallowed-registries) | Requires setting up allowed image sources (registries). Any other image source is disallowed. |

## Applying Policies
The policies can be created by applying the custom resources defined in the `template.yaml` and `contraint.yaml` files to an OpenShift cluster. The files are provided in each policy directory under specific security control.

For example, applying 'kubeadmin' temporary user removal validation policy

```
$ oc apply -f https://raw.githubusercontent.com/openshift-4-compliance/openshift-4-compliance-automation/master/open-policy-agent/authentication-user-management/delete-kubeadmin/template.yaml
$ oc apply -f https://raw.githubusercontent.com/openshift-4-compliance/openshift-4-compliance-automation/master/open-policy-agent/authentication-user-management/delete-kubeadmin/constraint.yaml
```

## Applying policies using GitOps
You can modify which policies to enforce/dryrun or include at all using kustomize.

**By default all policies are included as dryrun**.

### Import this kustomization
#### By extending this project
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- <PATH_TO_/open-policy-agent_FOLDER>
```

#### By importing this kustomization to your repo
```
---
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- https://github.com/openshift-4-compliance/openshift-4-compliance-automation.git//open-policy-agent?ref=<TAG>
```

### Modify this kustomization
#### Enforce specific policies
```
...

patchesJson6902:
- target:
    name: <POLICY_NAME>
  patch: |-
    - op: replace
      path: "/spec/enforcementAction"
      value: deny
```

#### Enforce a group of policies
```
...

patchesJson6902:
- target:
    labelSelector: policy-group=<GROUP_NAME>
  patch: |-
    - op: replace
      path: "/spec/enforcementAction"
      value: deny
```
#### Remove specific policies
```
...

patchesStrategicMerge:
- |-
  metadata:
    name: <POLICY_NAME>
  $patch: delete
```

#### Remove a group of policies
```
...

patchesStrategicMerge:
- |-
  metadata:
    labels:
      policy-group=<GROUP_NAME>
  $patch: delete
```