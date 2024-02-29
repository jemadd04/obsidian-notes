
Regardless of context, strive for...
- Scalability - the measure of a system's ability to handle varying amounts of work
	- Key business driver for migrating to the cloud
	- Reduce costs by removing under-utilized resources
	- When traffic is high, you want your application to be as quick as it is when traffic is low
- Resilience - ability for the system to continue to work even when one part of the system fails
	- Must test for failure
	- Best way to avoid failure is to introduce failure and learn from it; see what happens to your system
	- ABA = Always be Architecting!

Following business requirements
- Optimize all resources to the best of your ability
	- Ex: a non-critical computing job that needs to run every night. It would be cheaper and optimal to use Preemptible instances.
- Improve quality and availability of service. The higher the quality, the more attractive to users. 
- Maintain user experience at all times; meeting your SLO!
- Handle evolving market demands to continue growing

Upholding technology requirements
- DEVELOPMENT
	- Emphasis is on building *better* **faster**
	- Reduce toil through automation when possible
	- Increase feature development time
	- Follow the latest industry patterns and best practices; you can learn a lot by looking at new solutions to ongoing problems
- OPERATIONS
	- Automate to reduce frequency of failures
	- Bounce back as quickly as possible; reduce your mean time to recovery (MTTR)
	- Minimize failure of any single component so that it doesn't lead to the failure of the entire operation

Approximately 25% of the exam will be from case studies

Three major questions to ask to approach scenarios on the exam and in real life:
- Where is the company coming from?
	- Regardless of what solution you are architecting, take a hard look at the company's situation from business, technical, and personnel perspectives
- Where is the company going to?
	- Is the solution to be completely on GCP, hybrid, or multi-cloud? Is the market regional, national, or global?
	- Often dictates which service or services you should be selecting
- What's next?
	- If a case study says an org needs a solution for their current market but plans to expand to another market in the next year, it must be factored


### Summary
- Architect your solution to be scalable and resilient
- Business requirements often involve lowering costs while enhancing user experiences
- Keep an eye on technical needs during both development and operations
- Balance your solution on where a company is coming from, where it is going, and future developments