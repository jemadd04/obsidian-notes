
## In the console

Name: vm_name
	*Name for the VM instance*
Region: us-central1
Zone: us-central1-a
Series: E2
	*Name of the series*
Machine type: 2 vCPU
	*This is an e2-medium*
Boot disk: Debian GNU/Linux 11 (Bullseye); 10GB Balanced PD
Firewall: Allow HTTP traffic
	*Select this option in order to access a web server that you install later*

## In the CLI

gcloud compute instances create vm_name --machine-type e2-medium --zone=us-central1-a

SSH into the VM: gcloud compute ssh vm_name --zone=us-central1-a
Type 'y' to continue

### Installing an NGINX Web Server in GCP

1. SSH into the instance by clicking *SSH* on the VM instance
2. Update the OS: *sudo apt-get update*
3. Install NGINX: *sudo apt-get install -y nginx*
4. Confirm that NGINX is running: *ps auwx | grep nginx*
5. Click the External IP to see the default NGINX web page