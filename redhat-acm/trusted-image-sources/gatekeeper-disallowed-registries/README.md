# Gatekeeper Disallow Registries

The policy makes sure a list of "Allowed Registries" is associated with the [cluster Image resource](https://docs.openshift.com/container-platform/4.9/openshift_images/image-configuration.html). If a registry is not mentioned in the Image resource, images from this registry will not be pulled for pod creation.

The policy uses the next [Gatekeeper policy](../../../open-policy-agent/trusted-image-sources/disallowed-registries) in order to function.