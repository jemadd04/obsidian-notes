- Security Marks
	- Provides a way to annotate assets and then search, select, or filter using the mark via Cloud Security Command Center
	- Adds business context to assets for compliance
	- Enhances security focused insights into resources
	- Unique to Cloud Security Command Center
	- Set marks at the org level, project level, or for individual resources
	- Labels and network tags are also indexed by Cloud SCC
- Labels
	- Key-value pairs that help you organize your GCP resources
	- Attach a label to each resource, and then filter the resources based on their labels
	- Ex- environment = testing
	- Use cases
		- Identify individual teams or cost center resources
		- Distinguish deployment environments
		- Cost allocation and billing breakdowns
		- Monitor resource groups in the resource metadata
- Network Tags
	- Applied to instances and are the means for controlling network traffic to and from a VM instance
	- Identify VM instances that are subject to firewall rules and network routes
	- Use tags as source and destination values in firewall rules
	- Identify the instance on a certain route
	- Use gcloud commands, cloud console, or API calls to add or remove tags

### Choosing the right annotation

![[choosing-the-right-annotation.png]]



