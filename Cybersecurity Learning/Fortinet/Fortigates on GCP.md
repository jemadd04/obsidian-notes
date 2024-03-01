
FortiGate VM instances scale out automatically

This is achieved by using FortiGate native HA features such as config-sync, which synchronizes OS configs across multiple FortiGate VMs at time of scaleout events

### What is required to run FortiGates on GCP

- Firestore - stores autoscaling configuration such as primary and secondary IP addresses
- Cloud Storage bucket
- Cloud Functions - runs Fortinet-provided scripts for running auto scaling; used to handle cluster creation and failover management
- Managed instance group
- Instance template
- Cloud NAT