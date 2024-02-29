
### List active account name
*gcloud auth list*

### List project ID
*gcloud config list project*

### View project ID
*gcloud config get-value project*

### View project details
*gcloud compute project-info describe --project $(gcloud config get-value project)*

### Set project region
*gcloud config set compute/region REGION*
Ex: *gcloud config set compute/region us-central1*

### View project region
*gcloud config get-value compute/region*

### Set project zone
*gcloud config set compute/zone us-central1-a*

### View project zone
*gcloud config get-value compute/zone*

### Display gcloud components
*gcloud components list*

### List compute instances available in the project
*gcloud compute instances list*

### List specific VM
*gcloud compute instances list --filter="name=('[vm_name]')"*

### List project's firewall rules
*gcloud compute firewall-rules list*

### List firewall rules for the default network
*gcloud compute firewall-rules list --filter="network='default'"*

### Add a tag to a VM
*gcloud compute instances add-tags [vm_name] --tags http-server,https-server*

### Add a firewall rule
*gcloud compute firewall-rules create [name_of_rule] --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges:0.0.0.0/0 --target-tags=http-server*

### How to verify communication is possible for http to the VM
*curl http://$(gcloud compute instances list --filter=name:gcelab2 --format='value(EXTERNAL_IP)')*

### View system logs
*gcloud logging logs list*

### Read logs related to a resource type
*gcloud logging read "resource.type=gce_instance" --limit 5*

### Read logs for a specific VM
*gcloud logging read "resource.type=gce_instance AND labels.instance_name='gcelab2'" --limit 5*
