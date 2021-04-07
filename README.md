# OpenShift 4 Compliance Automation
A repository that aggregates policies from multiple OpenShift compliance products. The goal of the repository is to help achieve compliance with global OpenShift security standards, both in standalone OpenShift environments, and in multi cluster OpenShift environments.

The repository provides a framework for organization to build and use pre-configured policies. Using the provided policies, an organization is able to create a fine grained compliance standard in an environment based on OpenShift clusters.

## Repository Structure
The repository structure is based on security controls which are presented in the next OpenShift compliance document - [OCP 4.x Platform &Infrastructure Security Best Practices](https://github.com/rhilconsultants/openshift/blob/master/featureReference/Red%20Hat%20Openshift%204.x%20Security%20Best%20Practices%20-%20Public%20Edition%20-%20Final%20v2%20(2).pdf).

The compliance to the standard is achieved by using the next tools -
- [Red Hat Advanced Cluster Management for Kubernetes](https://github.com/open-cluster-management)
- [Open Policy Agent Gatekeeper](https://github.com/open-policy-agent/gatekeeper)

The repository's structure devides policies into one of the three mentioned above products. Each product's policies are devided into multiple compliance controls:
- authentication-user-management
- authorization
- etcd-security 
- infrastructure-general
- monitoring-observability
- networking 
- resource-exhaustion
- storage
- trusted-image-sources

Each product has a different method of deploying policies. Make sure that you follow the product's guidelines regarding proper deployment of policies.

## Status
Not all compliance controls have a corresponding policy. The policy development status can be found at - [Compliance Automation Sheets](https://docs.google.com/spreadsheets/d/1vhtWJYgvbVlW58D-jL9s-iBA56aT3m9Sf_qnME9gDyw/edit?usp=sharing)

## How to Contribute
If you would like to write a policy to the repository, make sure to fork the main repository, and commit your changes in your local repository. Afterwards, create a pull request and tag one of the project owners as a reviewer. The project owner will review the repository and merge the changes.
