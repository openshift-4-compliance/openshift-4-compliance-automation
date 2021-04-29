# Programming languages

Contents:

* [Summary](#summary)
  * [Issue](#issue)
  * [Decision](#decision)
  * [Status](#status)
* [Details](#details)
  * [Assumptions](#assumptions)
  * [Constraints](#constraints)
  * [Positions](#positions)
  * [Argument](#argument)
  * [Implications](#implications)
* [Related](#related)
  * [Related decisions](#related-decisions)
  * [Related requirements](#related-requirements)
  * [Related artifacts](#related-artifacts)
  * [Related principles](#related-principles)
* [Notes](#notes)


## Summary


### Issue

We need a flexible set of tools to allow the hardening of multiple OpenShift clusters according to the busniess security measures. The [OpenShift Compliance Operator](https://github.com/openshift/compliance-operator) can help aligning clusters' compliance state to common benchmarks such as [Center for internet Security (CIS)](https://www.cisecurity.org/). On top of that, an organization requires a specific set of security policies to be enforced on its Openshift clusters. The security policies should be enforced easily and modified as required over time. For that purpose, we have created the "Openshift 4 Compliance Automation" framework. The framework provides us with ready to go security templates that can be implemented in large organizations worldwide. The flexability of the framework allows its users to have their security requirements deployed in a form of custom policies. The policies are constantly monitored, and can be centraly managed in a multi cluster OpenShift environment using [Red Hat Advanced Cluster Management for Kubernetes](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/).

We need to choose the main tools we are going to use to create a compliance platform for users. The tools need to be Kubernetes native and apply to OpenShift API. The tools will be used to create custom compliance policies for OpenShift clusters. The policies will provide a customizable framework for organizations to maintain compliance.


### Decision

We are choosing Kyverno for declerative YAML policies for single managed OpenShift clusters.

We are choosing Red Hat Advanced Cluster Management for Kubernetes to create policies that apply to multi cluster OpenShift environments. In some cases Advanced Cluster Management policies will integrate with Open Policy Agent Gatekeeper in order to apply custom Admission policies.

Policies will be deployed using a GitOps fashion using Kustomize.

### Status

Decided. We are open to new alternatives as they arise.


## Details

### Assumptions

The compliance tools need to integrate with OpenShift API:

  * The tools will apply to OpenShift native resources.

  * The tools will be deployed using the `oc`, `kubectl` CLI or the Kubernetes / OpenShift user interface.

  * The tools will provide policies that are represented by a Kubernetes object.

The tools need to validate if Kubernetes objects follow a defined compliance standard:

  * We need to disallow Kubernetes objects that do not follow a policy.

  * We need to disallow creation of anti-pattern objects, and monitor pre-existing violating objects.

  * We need to apply a governance solution for OpenShift multi cluster scenarios.

We need a solution for a policy deployment strategy:

  * Each organization will decide which policies it needs to define.

  * The policy contents need to have **one** source of truth. e.g - Red Hat Advanced Cluster Management for Kubernetes policies that depend on Open Policy Gatekeeper policies will be changed when the Gatekeeper policy updates.

  * The deployment of policies will be done using the GitOps approach.

### Constraints

 * We have a strong constraint on tools that are usuable with OpenShift.

### Positions

We considered these tools:

 * OpenShift Compliance Operator

 * Open Policy Agent Gatekeeper

 * Kuberenetes Admission Controller Framework

 * Kyverno

 * Advanced Cluster Management for Kubernetes


### Argument

Summary per tool:

  * OpenShift Compliance Operator: Excellent for compliance against pre-configured banchmarks. e.g - CIS; However, it is not scalable and not modular enough for custom organization policies.

  * Open Policy Agent Gatekeeper: Provides a programable policy language (REGO) that evaluates Kubernetes Objects against the Admission Controller. The policy language (REGO) is hard to learn and master. The Gatekeeper operator can be integrated with Red Hat Advanced Cluster Management for multi cluster admission distribution.

  * Kubernetes Admission Controller Framework: Allows low level definitions of Admission Controllers. Rejected because it is not declerative and scalable enough. It consumes many resources to write custom Admission Controllers.

  * Kyverno: Provides a relatively new declaritive way of deploying policies. It allows policies to be declared in a native YAML format. However, the tool is new, and it is not proven to work with OpenShift yet. The tool does not integrate well with Red Hat Advanced Cluster Management for violation monitoring.

  * Advanced Cluster Management for Kubernetes: Allows deploying Kubernetes object onto multiple OpenShift cluster. It can use frameworks like Gatekeeper and Kyverno to implement policies.


We decided to create policies using _Kyverno_ to introduce both declerative and GitOps native approaches for policy deployment. Policies will be integrated with _Red Hat Advanced Cluster Management for Kubernetes_ for multi cluster policy disitribution.

We believe that our core decision is driven by two cross-cutting concerns:

  * For single cluster usecases, we will use declerative Kyverno policies.

  * Policies will be integrated to Red Hat Advanced Cluster Management for Kubernetes for multi cluster distribution. Multi cluster policies that require admission validation will use the Gatekeeper operator.


### Implications

* Going with Kyverno allows declaring policies in a native YAML format. The policies can be modified by updating the values in the YAML file that defines the policy.

* Going with Advanced Cluster Management for Kubernetes is only relevant when the organization manages multiple OpenShift clusters. Some of the Advanced Cluster Management for Kubernetes policies implement policies based on Open Policy Agent Gatekeeper.


## Related


### Related decisions

We will aim toward ecosystem choices that align with these tools and configured policies.


### Related requirements

Our entire toolchain must support these tools.


### Related artifacts

We expect we may use Kustomize to allow users descide which policies they would like to deploy in their environment.


### Related principles

* Security is at most importance. All policies must be tested for breaches. It is required to make sure that the policies work as intended.

* The policies can not protect a cluster administrator from himself. They are meant to prevent mistakes.

* The policies are modular. Each organization can mutate the policies as it wishes. Each policy can be modified and excluded accoriding to the organization's compliance regulations.


## Notes

Disclaimer - the policies are based on community contribution, and are not approved by a global security institute.
