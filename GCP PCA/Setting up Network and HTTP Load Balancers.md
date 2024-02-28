

Prior to creating the load balancers, we need something to actually load balance....so we will create multiple Compute Engine VM instances, install Apache on them, and then add a FW rule that allows HTTP traffic to reach each instance.

### Create the virtual machines: www1, www2, and www3

  gcloud compute instances create www1 \
    --zone=us-central1-a \
    --tags=network-lb-tag \
    --machine-type=e2-small \
    --image-family=debian-11 \
    --image-project=debian-cloud \
    --metadata=startup-script='#!/bin/bash
      apt-get update
      apt-get install apache2 -y
      service apache2 restart
      echo "
h3 Web Server: www1 h3" | tee /var/www/html/index.html'

  gcloud compute instances create www2 \
    --zone=us-central1-a \
    --tags=network-lb-tag \
    --machine-type=e2-small \
    --image-family=debian-11 \
    --image-project=debian-cloud \
    --metadata=startup-script='#!/bin/bash
      apt-get update
      apt-get install apache2 -y
      service apache2 restart
      echo "
h3 Web Server: www2 h3" | tee /var/www/html/index.html'

  gcloud compute instances create www3 \
    --zone=us-central1-a  \
    --tags=network-lb-tag \
    --machine-type=e2-small \
    --image-family=debian-11 \
    --image-project=debian-cloud \
    --metadata=startup-script='#!/bin/bash
      apt-get update
      apt-get install apache2 -y
      service apache2 restart
      echo "
h3 Web Server: www3 h3" | tee /var/www/html/index.html'

### Create a firewall rule to allow external traffic to VM instances

gcloud compute firewall-rules create www-firewall-network-lb --target-tags network-lb-tag --allow tcp:80

### Generate external IP addresses for the instances and verify they are running

gcloud compute instances list

curl http://[IP_of_vm1]
curl http://[IP_of_vm2]
curl http://[IP_of_vm3]


## Setting up a network load balancer

When configuring a LB service, the VM instances receive packets that are destined for the static external IP configured. Instances made with a Compute Engine image are automatically configured to handle this IP address.

Create a static external IP for the load balancer:
gcloud compute addresses create network-lb-ip-1 --region us-central1

Add a legacy HTTP health check:
gcloud compute http-health-checks create basic-check

Add a target pool in the **same region** as the instances. This command will create the target pool and use the health check created previously (which is required for the service to function):
gcloud compute target-pools create www-pool --region us-central1 --http-health-check basic-check

Add the instances to the target pool:
gcloud compute target-pools add-instances www-pool --instances www1,www2,www3

Add a forwarding rule:
gcloud compute forwarding-rules create www-rule \
	--region us-central1 \
	--ports 80 \
	--address network-lb-ip-1 \
	--target-pool www-pool

#### Now that the load balancer is configured, we can start sending traffic to the forwarding rule and observe the traffic being dispersed to the three instances

First, view the external IP of the forwarding rule created earlier that is being used by the load balancer and store it as a variable:
gcloud compute forwarding-rules describe www-rule --region us-central1
export IPADDRESS=$(gcloud compute forward-rules describe www-rule --region us-central1 --format="json" | jq -r .IPAddress)

Use the curl command to access the external IP:
while true; do curl -m1 $IPADDRESS; done
The responses should alternate between the three VMs
## Setting up an HTTP load balancer

HTTP Load balancing is implemented on Google Front End (GFE)
GFEs are distributed globally and operate together using Google's global network and control plane

Requests are always routed to the instance group closest to the user (as long as it has capacity and is appropriate for the request)
If it doesn't have capacity, the request is sent to the closest group that does have capacity

To create an HTTP LB, the instances must be in an instance group
Manage instance group provides VMs running the backend servers of an external HTTP load balancer. They let you operate apps on multiple identical VMs. You can make your workloads scalable and highly available by taking advantage of automated MIG services such as:
- Autoscaling
- Autohealing
- Regional (multi-zone) deployment
- Automatic updating

First we need to create the load balancer template:
gcloud compute instance-templates create lb-backend-template \
	--region=us-central1 \
	--network=default \
	--subnet=default \
	--tags=allow-health-check \
	--machine-type=e2-medium \
	--image-family=debian-11 \
	--image-project=debian-cloud \
	--metadata=startup-script='#!/bin/bash
	apt-get update
	apt-get install apache2 -y
	a2ensite default-ssl
	a2enmod ssl
	vm_hostname="$(curl -H "Metadata-Flavor:Google" \
	http://169.254.169.254/computeMetadata/v1/instance/name)"
	echo "Page served from: $vm_hostname" | \
	tee /var/www/html/index.html
	systemctl restart apache2'

Next we create a managed instance group using the template created above:
gcloud compute instance-groups managed create lb-backend-group \
	--template=lb-backend-template
	--size=2
	--zone=us-central1-a

Create a firewall rule to allow health check traffic:
gcloud compute firewall-rules create fw-allow-health-check \
  --network=default \
  --action=allow \
  --direction=ingress \
  --source-ranges=130.211.0.0/22,35.191.0.0/16 \
  --target-tags=allow-health-check \
  --rules=tcp:80

Now that we have the instances up and running we need to set up a global static external IP address that customers will use to reach the load balancer:
gcloud compute addresses create lb-ipv4-1 \
	--ip-version=IPV4 \
	--global

Create a health check for the load balancer:
gcloud compute health-checks create http http-basic-check --port 80

Next we create a backend service:
gcloud compute backend-services create web-backend-service \
  --protocol=HTTP \
  --port-name=http \
  --health-checks=http-basic-check \
  --global

Add the instance group as the backend to the backend service:
gcloud compute backend-services add-backend web-backend-service \
  --instance-group=lb-backend-group \
  --instance-group-zone=us-central1-a \
  --global

Now we will create a URL map to route the incoming requests to the default backend service. URL map is a Google Cloud configuration resource used to route requests to backend services or backend buckets.
gcloud compute url-maps create web-map-http --default-service web-backend-service

Create a target HTTP proxy to route requests to the URL map:
gcloud compute target-http-proxies create http-lb-proxy --url-map web-map-http

Create a global forwarding rule to route incoming request to the proxy. This represents the frontend configuration of the load balancer:
gcloud compute forwarding-rules create http-content-rule \
   --address=lb-ipv4-1\
   --global \
   --target-http-proxy=http-lb-proxy \
   --ports=80

That's it! Now to test the traffic, go into the console and click on the LB created (in this case, web-map-http). In the backend section click on the name of the backend and confirm that the VMs are **healthy**. 
When healthy, test the LB in the web browser by going to http://[LB_IP_ADDRESS]
Note: Could take 3-5 minutes