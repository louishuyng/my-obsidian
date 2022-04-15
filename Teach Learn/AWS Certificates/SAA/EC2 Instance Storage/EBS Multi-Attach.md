
- Attach the same EBS volume to multiple EC2 instances in the same AZ
- Use case:
	- Achive **higher application availability** in clustered Linux applications (Ex: Teradata)
	- Applications must manage concurrent write operations
- Must use a file system that's cluster-aware (not XFS, EX4, etc...)
- ![[Screen Shot 2022-04-07 at 23.40.09 1.png]]