- Managed NFS (network file system) that can be mounted on many EC2
- EFSs works with EC2 instances in multi-AZ
- Highly available, scalable, expensive (3x gp2), pay per use
- ![[Screen Shot 2022-04-09 at 14.11.13.png]]
- Use cases: content management, web serving, data sharing, Wordpress
- Uses NFSv4.1 protocol
- Uses security group to control access to EFS
- Compatible with Linux based AMI (not windows)
- Encryption at rest using KMS
- POSIX file system (~ Linux) that has a standard file API
- File system scales automatically, pay per use, no capacity planning!

### Performance & Storage Classes
- EFS Scale
	- 1000s of concurrent NFS clients, 10 GB+ /s throughput
	- Grow to Petabyte-scale network file system automatically
- Performance mode (set at EFS creation time)
	- General purpose (default): latency-sensitive use cases (web server, CMS, etc...)
	- Max I/O - higher latency, throughput, highly parallel (big data, media processing)
- Throughput mode
	- Bursting (1 TB = 50 Mib/s + burst of up to 100 Mib/s)
	- Provisioned: set your throughput regardless of storage size, ex: 1 Gib/s for 1 TB storage (fixed)
### Storage Classes
- Storage Tiers (life cycle managment feature - move file after N days)
	- Standard: for frequently accessed file
	- Infrequent access (EFS-IA): cost to retrieve files, lower price to store. Enable EFS-IA with a Lifecycle Policy![[Screen Shot 2022-04-09 at 14.24.17.png]]
- Availability and durability
	- Regional: Multi-AZ, great for prod
	- One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA) Over 90% in cost savings