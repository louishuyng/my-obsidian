- Sometimes you want control over the EC2 Instance placement strategy
- That strategy can be defined using placement groups
- When you create a placement group, you specify one of the following strategies for the group:
	- Cluster - clusters instances into a low-latency group in a single Availability Zone
	- Spread - spreads instances across underlying hardware (max **7 instances** per group per AZ) - critical applicaations
	- Partition - spreads instances across many different partions (which rely on different sets of racks) within  an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)

### Cluster
![[Screen Shot 2022-04-03 at 11.52.57 1.png]]
- Use case:
	- Big Data job that needs to complete fast
	- Application that needs extremely low latency and high network throughput
### Spread
![[Screen Shot 2022-04-03 at 11.56.09.png]]
-  Use case:
	- Application that needs to maximize high availability
### Partition
![[Screen Shot 2022-04-03 at 11.59.02.png]]
- Use case:
	- when you have an application that it can be partition aware to distribute the data
	- big data applications which are partition aware such as HDFS, Hbasem Cassandra and Apache Kafka