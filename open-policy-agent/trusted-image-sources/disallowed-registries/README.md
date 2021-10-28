# Disallow Registries

The policy makes sure a list of "Allowed Registries" is associated with the `cluster` Image resource. If a registry is not mentioned in the Image resource, images from this registry will not be pulled for pod creation.

Managing a list of allowed registries provides control over what code runs on the OpenShift cluster. Anomalous registry instances can contain dangerous unscanned images in them, such images must be avoided.

A list of allowed registries is created in the [constraint.yaml](constraint.yaml) file. If an Image resource has not allowed registries associated with it, an alert is initiated by the constraint. An alert is initiated if there are no registries configured in the Image resource as well.

`This policy has been tested on openshift cluster & oc client version 4.9.0`