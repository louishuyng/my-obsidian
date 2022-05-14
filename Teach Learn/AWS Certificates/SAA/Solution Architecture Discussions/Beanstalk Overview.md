![[Screen Shot 2022-05-14 at 15.27.21.png]]

Developer problems on AWS
- Managing infrastructure
- Deploying Code
- Configuring all the databases, load balancers, etc
- Scaling concerns

- Most web apps have the same architecture (ALB + ASG)
- All the developers want is for their code to run!
- Possibly, consistently across different applications and environments

### Overview
- Elastic Beanstalk is a developer centric view of deplyoing an application on AWS
- It uses all the component's we've seen before: EC2, ASG, ELB, RDS, ...
- Managed service
	- Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration, ...
	- Just the application code is the responsibility of the developer
- We still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances
### Components
- **Application**: collection of Elastic BEenstalk components (environments, versions, configurations, ...)
- **Application Version**: an iteration of your application code
- **Environment**
	- Collection of AWS resources running an application version (only one application version at a time)
	- **Tiers**: Web Server Environment Tier & Worker Environment Tier
	- You can create multiple environments (devm test, prod, ...)
	- ![[Screen Shot 2022-05-14 at 15.37.22.png]]
### Supported Platforms
- Go
- Ruby
- Java SE
- Java with Tomcat
- .NET Core on Linux
- .NET on Windows Server
- Node.js
- PHP
- Python
- Packer Builder
- Single Container Docker
- Multi-Continer Docker
- Preconfigured Docker

- If not supported you can write your custom platform (advanced)

### Web Server Tier vs Worker Tier
![[Screen Shot 2022-05-14 at 15.41.50.png]]