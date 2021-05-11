# Contributing policies

This doc provides instruction on how to contribute policies to the `openshift-4-compliance-automation` repository.

You can contribute policies by submitting a pull request (PR) in the `openshift-4-compliance-automation` repository. Your PR must be reviewed by the owners of the repo. The repository is welcoming every contributor to join the project and create an automated compliance implementation for OpenShift environments.

## Guidelines

The repository is based on 3 main tools -

- Open Policy Agent Gatekeeper
- Kyverno
- Red Hat Advanced Cluster Management for Kubernetes

Each tool will be associated with its own directory. 

Each product's policies are devided into multiple compliance controls -

- authentication-user-management
- authorization
- etcd-security 
- infrastructure-general
- monitoring-observability
- networking 
- resource-exhaustion
- storage
- trusted-image-sources

Each compliance control will have its own directory in each tool directory. The compliance control directories are based on the next [doc](https://docs.google.com/spreadsheets/d/1vhtWJYgvbVlW58D-jL9s-iBA56aT3m9Sf_qnME9gDyw/edit?usp=sharing).

The directory structure is constant, and should not be changed.

## Contributing a policy

Contributing custom policies can be done to every tool and compliance control in the list. The policy must follow the below rules in order to be approved.

Creating a custom policy requires you to fork the repository to your GitHub account. Afterwards, clone the repository to your local machine -

```
$ git clone https://github.com/<your-username>/openshift-4-compliance/openshift-4-compliance-automation.git
```

### Open Policy Agent Gatekeeper policies

- In order to create an Open Policy Agent Gatekeeper policy, create a new directory under the relevant compliance control in the [open-policy-agent directory](../open-policy-agent). Make sure to create a readable name to the policy directory.

```
$ mkdir ./open-policy-agent/networking/httpsonly
```

- Create a `template.yaml` and `constraint.yaml` files. The files will contain the policy definition. 

```
$ ls -l ./open-policy-agent/networking/httpsonly/
-rw-rw-r--. 1 mkotelni mkotelni 199 Apr 29 16:30 constraint.yml
-rw-rw-r--. 1 mkotelni mkotelni 802 Apr 29 16:30 template.yml
```

- Create a `README.md` file that briefly explains the policy, and how to use it. Instructions on how to create a `README.md` file for policies can be found [here](./).

```
$ ls -l ./open-policy-agent/networking/httpsonly/
-rw-rw-r--. 1 mkotelni mkotelni 199 Apr 29 16:30 constraint.yaml
-rw-rw-r--. 1 mkotelni mkotelni 802 Apr 29 16:30 template.yaml
-rw-rw-r--. 1 mkotelni mkotelni 154 Apr 29 16:30 README.md
```

- Add the policy to the relevant table in the [README.md](./open-policy-agent/README.md) file.

```
### Networking
Policy  | Description | Prerequisites
------- | ----------- | -------------
[httpsonly](./networking/httpsonly) | Ensures that there are *no* http routes |
```

### Kyverno

- In order to create a Kyverno policy, create a new directory under the relevant compliance control in the [kyverno directory](../kyverno). Make sure to create a readable name for the policy directory.

```
$ mkdir ./kyverno/networking/httpsonly
```

- Create a `<policy-name>.yaml` file. The file will contain the policy definition. Make sure to create a readable name for the policy.

```
$ ls -l ./kyverno/networking/httpsonly/
-rw-rw-r--. 1 mkotelni mkotelni 199 Apr 29 16:30 httpsonly.yaml
```

- Create a `README.md` file that briefly explains the policy, and how to use it. Instructions on how to create a `README.md` file for policies can be found [here](./).

```
$ ls -l ./kyverno/networking/httpsonly/
-rw-rw-r--. 1 mkotelni mkotelni 199 Apr 29 16:30 httpsonly.yaml
-rw-rw-r--. 1 mkotelni mkotelni 154 Apr 29 16:30 README.md
```

- Add the policy to the relevant table in the [README.md](./kyverno/README.md) file.

```
### Networking
Policy  | Description | Prerequisites
------- | ----------- | -------------
[httpsonly](./networking/httpsonly/httpsonly.yaml) | Ensures that there are *no* http routes |
```

### Red Hat Advanced Cluster Management for Kubernetes

- In order to create a Red Hat Advanced Cluster Management policy, create a new directory under the relevant compliance control in the [redhat-acm directory](../redhat-acm). Make sure to create a readable name for the policy directory.

```
$ mkdir ./redhat-acm/networking/gatekeeper-allow-httpsonly
```

- Create a `<policy-name>.yaml` file. The file will contain the policy definition. Make sure to create a readable name for the policy.

```
$ ls -l ./redhat-acm/networking/gatekeeper-allow-httpsonly/
-rw-rw-r--. 1 mkotelni mkotelni 199 Apr 29 16:30 gatekeeper-allow-httpsonly.yaml
```

- Create a `README.md` file that briefly explains the policy, and how to use it. Instructions on how to create a `README.md` file for policies can be found [here](./).

```
$ ls -l ./redhat-acm/networking/gatekeeper-allow-httpsonly/
-rw-rw-r--. 1 mkotelni mkotelni 199 Apr 29 16:30 gatekeeper-allow-httpsonly.yaml
-rw-rw-r--. 1 mkotelni mkotelni 154 Apr 29 16:30 README.md
```

- Add the policy to the relevant table in the [README.md](./redhat-acm/README.md) file.

```
### Networking
Policy  | Description | Prerequisites
------- | ----------- | -------------
[gatekeeper-allow-httpsonly](./networking/gatekeeper-allow-httpsonly/gatekeeper-allow-httpsonly.yaml) | Ensures that there are *no* http routes | The [GateKeeper](https://github.com/open-policy-agent/gatekeeper) operator needs to be installed
```

### Contributing

Create a pull request that will be reviewed by the team. Run the next commands on the repository you forked in the previous steps.

1. Add all changed files to the commit.

```
$ git add -A
```

2. Commit all changed files with a descriptive message.

```
$ get commit -m "Added a policy that disables http routes from being created"
```

3. Push the changes to your remote fork.
```
$ git push origin master
```

4. Follow the "Pull Request Guidelines" below in order to create a Pull Request in the repository.

## Pull Request Guidelines

### Contributing new policy
- Open a new GitHub pull request with the policy.
- Only one policy should be submitted in a single pull request.
- Ensure PR description clearly describes the policy. Include the relevant [issue](https://github.com/openshift-4-compliance/openshift-4-compliance-automation/issues) number if applicable.
- Submit the policy after appropriate testing is done and the policy behaves as expected.
- Include only files that are relevant to the policy and matches PR description.
- Assign a reviewer.
- Before submitting, please be familiar with [contributing policies](./docs/CONTRIBUTING.md) guide.

### Reporting Bugs
- Ensure the bug was not already reported by searching on GitHub under [issues](https://github.com/openshift-4-compliance/openshift-4-compliance-automation/issues).
- If you're unable to find an open issue addressing the problem, [open a new one](https://github.com/openshift-4-compliance/openshift-4-compliance-automation/issues/new). Be sure to include a title
  and clear description, and a clear example of the issue.
  
### Fixing existing policy
- Ensure PR description clearly describes the policy. Include the relevant [issue](https://github.com/openshift-4-compliance/openshift-4-compliance-automation/issues) number if applicable.
- Include only changes in files that are relevant to the policy and matches PR description.
- Assign a reviewer.




