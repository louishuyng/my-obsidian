 
- Load Balances are servers that forward traffic to multiple servers (e.g, EC2 instances) downstream
 ![[Screen Shot 2022-04-15 at 10.26.33.png]]
### Why use a load balancer ?
- Spread load accross multiple downstream instances
- Expost a single point of access (DNS) to your application
- Seamlessly handle failures of downstream instances
- Do regular health checks to your instances
- Provide SSL termination (HTTPs) for your website
- Enforce stickiness with cookies
- High availability across zonens
- Seperate public traffic from private traffic

#### AWS Elastic Load Balancers is a managed load balancer
- AWS gurantess that it will be working
- AWS takes care of upgrades, maintenance, high availability
- AWS provides only a few configuration knobs
- It costs less to setup your own load balancer but it will be a lot more effort on your end
- It is integrated with many AWS offerings/ services
	- EC2, EC2 Auto Scaling Groups, Amazon ECS
	- AWS Certificate Manager (ACM), CloudWatch
	- Route 53, AWS WAF, AWS Global Accelerator
### Health Checks
- are crucial for Load Balancers
- they enable the load balancer to know if instances it forwards traffic to are available to reply to requests
- The health check is done on a port and a route (/heath is common)
- If the response is not 200 (OK), then the instance is unhealthy
### Types of load balancer on AWS
- [[Classic Load Balancer (CLB)]]
- [[Application Load Balancer (ALB)]]
- [[Network Load Balancer (NLB)]]
- [[Gateway Load Balancer (GLB)]]

### Load Balancer Security Group
![[Screen Shot 2022-04-15 at 11.01.28.png]]