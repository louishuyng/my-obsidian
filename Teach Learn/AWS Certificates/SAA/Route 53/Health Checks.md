- HTTP Health Checks are only for **public resources**
- ![[Screen Shot 2022-05-01 at 18.18.02.png]]
- Heath Check => Automated DNS Failover:
	- Health checks that monitor an endpoint (application, server, other AWS resource)
	- Heath checks that monitor other health checks (Calculated Health Checks)
	- Heath cheks that monitor CloudWatch Alarms (full control !!) - eg, throttles of DynamoDB, alarms on RDS, custom metrics, ... (helpful for private resources)
- Heath Checks are integrated with CloudWatch metrics

### Montior an Endpoint
- About 15 global health checkers will check the endpoint health
	- Healthy/Unhealthy threshold - 3 (default)
	- Interval - 30 sec (can set to 10 sec - higher cost)
	- Supported protocol: HTTP, HTTPS and TCP
	- If > 18% of health checkers report the endpoint is healthy, Route 53 considers it Healthy. Otherwise, it's Unhealthy
	- Ability to choose which locations you want Route 53 to use
- Heath Checks pass only when the endpoint responds with the 2xx and 3xx status codes
- Heath Checks can be setup to pass / fail based on the text in the first **5120 bytes** of the response
- Configure you router/firewall to allow incoming requests from Route 53 Health Checkers
- ![[Screen Shot 2022-05-01 at 18.21.41.png]]

### Calculated Health Checks
- Combine the results of multiple Health Checks into a single Health Check
- You can use OR, AND, or NOT
- Can monitor up to 256 Child Health Checks
- Specify how many of the health checks need to pass to make the parent pass
- Usage: perform maintenance to your website without causing all health checks to fail
- ![[Screen Shot 2022-05-01 at 19.29.34.png]]

### Private Hosted Zones
- Route 53 health checkers are outside the VPC
- They can't access **private** endpoints (private VPC or on-premises resource) 
- You can create a **CloudWatch Metric** and associate a **CloudWatch Alarm**, then create a Health Check that  checks the alarm itself
- ![[Screen Shot 2022-05-01 at 19.36.55.png]]