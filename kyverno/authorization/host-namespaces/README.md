# Host Namespaces policies

Policies placed under directory 'host-namespaces' targets to disable pods access to sensitive host's resources.

Access to several host namespaces (Process ID namespaces, Inter-Process Communication namespaces, and network namespaces) allows access to shared information and can be used to elevate privileges. 
Access to host ports allows potential snooping of network traffic.
Access to Host path volumes lets pods use host directories and volumes in containers.

Each policy targets non-system pods (Pods that are running outside of 'openshift-*' namespaces') and non-default SCCs.



