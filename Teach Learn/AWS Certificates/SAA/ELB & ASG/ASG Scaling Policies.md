- Target Tracking Scaling
	- Most simple and easy to set-up
	- Example: I want the average ASG CPU to stay at around 40%
- Simple / Step Scaling
	- When a CloudWatch alarm is triggered (example CPU > 70%) then add 2 units
	- When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1 unit
- Scheduled Actions
	- Anticipate a scaling based on know usage patterns
	- Example: increase the min capacity to 10 at 5 pm on Fridays
- Predictive scaling: continously forecast load and schedule scaling ahead
	- ![[Screen Shot 2022-04-21 at 22.06.45.png]]
### Good metrics to scale on
- CPU Utilization: Average CPU utilization across your instances
- RequestCountPerTarget: to make sure the number of requests per EC2 instances is stable![[Screen Shot 2022-04-21 at 22.09.39.png]]
- Average Network In/ Out (if you're application is network bound)
- Any custom metric (that you push using CloudWatch)
### Scaling Cooldowns
- After a scaling activiy happens, you are in the cooldown period (default 300 seconds)
- During the cooldown period, the ASG will not launch or terminate additonal instances (to allow for metrics to stabilize)![[Screen Shot 2022-04-21 at 22.11.53.png]]
- Adivce: Use a ready-to-user AMI to reduce configuration time in order to be serving request fasters and reduce the cooldown period