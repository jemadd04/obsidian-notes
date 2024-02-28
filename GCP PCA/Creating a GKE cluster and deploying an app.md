
Some of the benefits of running a GKE cluster:
- Load balancing for Compute Engine instances
- Node pools to designate subnets of nodes within a cluster for additional flexibility
- Automatic scaling of your cluster's node instance count
- Automatic upgrades for your cluster's node software
- Node auto-repair to maintain node health and availability
- Logging and monitoring with Cloud Monitoring for visibility into your cluster

A cluster consists of at least one **cluster master** machine and multiple worker machines called **nodes**

The nodes are Compute Engine VMs that run the K8s processes necessary to make them part of the cluster

## Creating a GKE Cluster

*gcloud container clusters create --machine-type=e2-medium --zone=us-east1-d [cluster-name]
Note: Will take several minutes to complete, so be patient!

## Deploying an application to the cluster

Before you can interact with the cluster, you need to authenticate with it:
*gcloud container clusters get-credentials lab-cluster*

To create a new deployment with a container image run the following kubectl create command. The image is pulled from an existing Container Registry bucket:
*kubectl create deployment [deployment-name] --image=gcr.io/google-samples/hello-app:1.0*

We now want to expose the application to external traffic. Do this with the following kubectl expose command:
*kubectl expose deployment hello-server --type=LoadBalancer --port 8080*
This command 1) specifies the port that the container exposes and 2) creates a Compute Engine load balancer for the container. It will take a minute for the external IP of the LB to generate. Run *kubectl get service* to check the status.

To view the application from a web browser:
http://[LB-EXTERNAL-IP]:8080
## Deleting the cluster

Delete a cluster with:
*gcloud container clusters delete [cluster-name]*
Type Y to confirm when prompted.
Note: Will take a few minutes.