### ASG Default Termination Policy (simplified version)
1. Find the AZ which has the most number of instances
2. If there are multiple instances in the AZ to choose from, delete the one with the oldest launch configuration
- ASG tries the balance the number of instances across AZ by default
- ![[Screen Shot 2022-04-21 at 23.05.12.png]]
### Lifecycle Hooks
- By default as soon as an instnace is launched in an ASG it's in service
- You have the ability to perform extra steps before the instance goes in service (Pending state)
- You have the ability to perform some actions before the instance is terminated (Terminating state)
- ![[Screen Shot 2022-04-21 at 23.07.49.png]]
### Launch Template vs Launch Configuration
- Both:
	- ID of the Amazon Machine Image (AMI), the instance type, a key pair, security groups, and the other parameters that you use to launch EC2 instances (tags, EC2 user-data...)
-  Launch Configuration (legacy):
	- Must be re-created every time
- Launch Template (newer):
	- Can have multiple versions
	- Create parameters subsets (partial configuration for re-user and inheritance)
	- Provision using both On-Demand and Spot instances (or aa mix)
	- Can use T2 unlimited burst feature
	- Recommended by AWS going forward