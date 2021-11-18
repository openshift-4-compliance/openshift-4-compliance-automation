# Deploying policies using the OpenShift Compliance Automation

This section describes how you can use the framework to deploy policies in the repository.

### Red Hat Advanced Cluster Management for Kubernetes

Before you deploy policies using RHACM, run the next command to allow policy creation via GitOps (change <your-username> to the user used to run the command) -

```
<hub> $ oc patch clusterrolebinding.rbac open-cluster-management:subscription-admin -p '{"subjects": [{"apiGroup":"rbac.authorization.k8s.io", "kind":"User", "name":"<your-username>"}]}'
```

Deploy all policies in the repository -

```
<hub> $ oc apply -f rhacm-policies.yaml
```

By running the command you are deploying all RHACM policies in the repository. Any change in the repository is automatically reflected in your environment. You may fork this repository and modify the [rhacm-policies.yaml](rhacm-policies.yaml) file to create your own version of the policy repository.