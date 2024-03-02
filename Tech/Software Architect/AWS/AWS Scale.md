---
tags:
  - Architect
  - AWS
---

Once upon a time, there lived 2 software engineers named James and Robert.

They worked for a tech company named Hooli.

Although they were bright engineers, they never got promoted.

So they were sad and frustrated.

![AWS Scale](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5c55996-9355-40c1-8f65-22b617c51ff5_1200x630.gif "AWS Scale")

Until one day when they had a smart idea to build a startup.

Their growth rate was mind-boggling.

Yet they wanted to keep it simple. So they hosted the app on AWS.

---

## AWS Scale

Here’s their scalability journey from 0 to 10 million users:

### 1. Prelaunch

Their minimum viable product (**MVP**) was not production-ready.

So they used a static framework to build the launch page. And served static pages at no operational costs via AWS Amplify hosting.

_AWS Amplify_ is a web app hosting service.

![AWS Scale; Static hosting](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F281ad72a-a6ce-4dea-85e1-800704f3160e_1585x468.png "AWS Scale; Static hosting")


Launch Page on Static Hosting

Also they added simple backend code in AWS Lambda.

_AWS Lambda_ is a serverless platform. It allowed them to run code without managing servers.

j### 2. Launch

They launched the MVP.

But they had only a handful of users.

So they used a single EC2 instance to run the entire web stack: database and backend.

_Amazon Elastic Compute Cloud_ (**EC2**) is a virtual server.

They attached an elastic IP address to the EC2 instance and then pointed Amazon Route 53 _DNS_ toward it.

An **Elastic IP address** is a static IP address.


![AWS scale; web stack on EC2](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F83090fa5-61a4-4a42-a87b-97270009b52a_1727x462.png "AWS scale; web stack on EC2")



Yet they hit the storage limits because some users had more data.

So they installed a larger instance type to scale vertically.

jIncreasing the capacity of a single server for performance is called **Vertical Scaling**.

The configuration template for virtual servers is called **Instance Type**. For example, _nano_ and _micro_ instance types offer varying CPU, memory, and storage.

But there is a risk of a single point of failure with vertical scaling. Also a service cannot be scaled vertically beyond a limit.

### 3. Ten Users

But one day. They hit the memory limit with the single EC2 instance.

So they moved the backend and database into separate EC2 instances.

And run the database on a larger instance type.

![AWS scale; EC2 instances running app services](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff801d5aa-ef35-4462-8544-880d65eb01f2_726x462.png "AWS scale; EC2 instances running app services")

Backend and Database Running on Separate EC2 Instances

They used an SQL database because it was enough to handle the first 10 million users. Also it offered them strong community support and tools. And life was good.

### 4. Thousand Users

Until one day when they received a phone call.

There was a power outage in the availability zone hosting their servers.

So they installed an extra server in another availability zone.

And set up the database in the leader-follower replication topology.

The database follower ran in another availability zone.

The database leader served the write operations. While the database follower served the reads.

Also they set up automatic failover for high availability.

![AWS Scale; High availability with 2 AZ](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8547e73e-c809-492e-bcc9-e143b673477a_1173x1174.png "AWS Scale; High availability with 2 AZ")



Running the App Across Two Availability Zones

They load-balanced the traffic between 2 availability zones.

And pointed DNS toward the load balancer.

They used an Elastic Load Balancer (**ELB**). And it did server health checks to prevent traffic to failed instances.

### 5. Ten Thousand Users

But one day.

They hit the memory limit again. And servers ran at full capacity.

They wanted to scale out.

So they made the backend stateless by moving out the session state. And storing it in ElasticCache.

Besides they added more servers behind the load balancer to scale horizontally. And installed extra database followers.

![AWS Scale; stateless app](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F800e265f-1472-47fe-be30-b4546d78654c_1016x217.png "AWS Scale; stateless app")


Making App Server Stateless by Storing State in Cache

Yet the database reads were becoming a bottleneck.

So they cached the popular database reads in ElasticCache.

_ElasticCache_ is a managed Memcached or Redis.

![AWS scale; caching reads](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ab3c2f8-f968-4c26-970f-1daaecd70618_1021x491.png "AWS scale; caching reads")


Caching Database Reads to Reduce Load

Also they moved static content to Amazon S3 and CloudFront to reduce the server load. The static content is CSS, images, and JavaScript files.

_Amazon S3_ is an object store. While _CloudFront_ is a Content Delivery Network (**CDN**).

![AWS scale; CDN with static content](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4bbbc36d-47a3-4876-a944-b13a3bdfbee2_1241x315.png "AWS scale; CDN with static content")


CDN Hosting Static Content

They reduced latency by caching data in CDN at edge locations.

A content delivery endpoint in a CDN network is called an **Edge Location.**

![AWS scale; Autoscaling](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fed271fac-fefb-48ad-afef-d23f939ff53b_1195x898.png "AWS scale; Autoscaling")


Dynamically Scaling Servers With Autoscaling

Yet one day.

They noticed their peak traffic was much higher than average traffic.

So they over-provisioned servers to handle peak traffic.

But it cost them a lot of money.

So they set up autoscaling with CloudWatch.

Scaling the computing resources based on load is called **Autoscaling**. It reduced their costs and operational complexity.

_CloudWatch_ is Amazon's monitoring and management service. It embeds an agent in each service to collect metrics.

### 6. Half a Million Users

Their growth was inevitable.

And they wanted to offer the users a service level agreement (**SLA**) of 99.99% high availability.

Put another way, they couldn’t tolerate downtime of more than 52 minutes per year.

So they replicated the servers across many availability zones. And offloaded session data to DynamoDB.

Besides they ran each server at full capacity for performance.

_DynamoDB_ is a managed NoSQL database.

![AWS scale; microservices architecture](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd98b7829-005f-4ffb-9e04-ea7c4aa6fa04_1505x686.png "AWS scale; microservices architecture")



Monolith vs Microservices

They moved to a microservices architecture because wanted to scale services independently.

![AWS scale; load balancer](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6b25db77-c37d-4d9d-94e9-27013e86202c_1356x222.png "AWS scale; load balancer")


Load Balancing Different Layers

Also they added load balancers between each layer to scale out.

![AWS Scale; CloudFormation](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44b92709-fc18-4dee-ace8-9c7679a96f78_1991x285.png "AWS Scale; CloudFormation")

CloudFormation Workflow

They used AWS CloudFormation to create a templatized view of the web stack. Because it reduced the operational complexity.

_CloudFormation_ is an infrastructure as a code deployment service.

And life was good again.

### 7. Ten Million Users

But one day.

They noticed database writes being slow due to data contention.

So they federated the database and then sharded it.

![AWS Scale; Federation](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F650cb4b6-e955-4816-8c31-c7bd21aaa311_853x244.png "AWS Scale; Federation")


Database Federation

Splitting a database into many databases based on the business domain is called **Federation**. For example, storing product and user information in separate databases.

Federation allowed them to scale easily.

Yet it became difficult to query cross-function data due to many databases.

![AWS Scale; Sharding](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa76d1b62-88b5-4a2f-ad91-d6f18dc6f9c4_1180x295.png "AWS Scale; Sharding")

Replication vs Sharding

Splitting a single dataset across many servers is called **Sharding**.

They used sharding to scale out.

But it made the application layer more complex because the data needed to be joined at the application level.

Also they moved the data that doesn't need complex joins to a NoSQL database.

![AWS Scale; multi region scalability](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F292d0562-b78f-4fe1-936b-1b6df7a69c78_800x500.gif "AWS Scale; multi region scalability")


Besides they extended the system across many AWS regions. Because it offered high availability and scalability.

And lived happily ever after.

---

AWS has 32 regions and 102 availability zones around the world.

![AWS Scale; regions and availability zones](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb37f946-22c9-41f3-b7fd-45ea80f21dd2_1418x438.png "AWS Scale; regions and availability zones")

AWS Regions and Availability Zones

A Geographical area with data centers is called an **AWS Region**. Each region has more than a single availability zone.

A single data center or a combination of many data centers within an AWS region is called an **Availability Zone**.

Each AZ is connected to a separate network and power for high availability.