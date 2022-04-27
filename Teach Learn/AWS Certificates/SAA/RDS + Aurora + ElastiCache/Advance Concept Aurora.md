### Auto Scaling
![[Screen Shot 2022-04-26 at 11.19.06.png]]

### Custom Endpoints
![[Screen Shot 2022-04-26 at 11.20.15.png]]

### Aurora Serverless
- Automated database instantiation and auto-scaling based on actual usage
- Good for infrequent intermittent or unpredictable 
- No capacity planning needed
- Pay per second, can be more cost-effective
- ![[Screen Shot 2022-04-26 at 11.41.22.png]]

### Multi-Master
- In case you want immediate failover for write node (HA)
- Every node does R/W - vs promoting a RR as the new master
- ![[Screen Shot 2022-04-26 at 11.43.16.png]]

### Global Aurora
- Aurora Cross Region Read Replicas:
	- Useful for disaster recovery
	- Simple to put in place
- Aurora Global Database (recommended)
	- 1 Primary Region (read / write)
	- Up to 5 secondary (read-only) regions, replication lag is less than 1 second
	- Up to 16 Read Replicas per secondary region
	- Helps for decreasing latency
	- Promoting another region (for disaster recovery) has an RTO (Recovery time objective) of < 1 minute

### Machine Learning
- Enables you to add ML-based predictions to your applications via SQL
- Simple, optimized, and secure integration between Aurora and AWS ML services
- Supported services
	- Amazon SageMaker (use with any ML model)
	- Amazon Comprehend (for sentiment analysis)
- You don't need to have ML exprience
- Use cases: fraud detection, ads targeting, sentiment analysis, product recommendations
-  ![[Screen Shot 2022-04-26 at 23.43.00.png]]
