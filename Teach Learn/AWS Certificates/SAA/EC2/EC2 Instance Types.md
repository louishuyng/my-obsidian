Instance Types: https://aws.amazon.com/ec2/instance-types

`m5.2xlarge`

AWS has the following name convention:
- m: instance class
- 5: generation (AWS improves them over time)
- 2xlarge: size within the instance 

## Types
### General Purpose
- Greate for diveristy of workloads such as web servers or code repositories
- Balance between:
	- Compute
	- Memory
	- Networking

### Compute Optimized
- Great for compute-intensive tasks that require high performance processors:
	- Bath processing workloads
	- Media transcoding
	- High performance web servers
	- High performance computing (HPC)
	- Scientific modeling & machine learning
	- Dedicated gaming servers
- Example: `C6g`, `C5`

###  Memory Optimized
- Fast perofmance for workloads that process large data sets in memory
- Use cases:
	- High performance, relational/ non-relational database
	- Distributed web scale cache storages
	- In-memory databases optimized for BI (business intelligence)
	- Applications performing real-time processing of big unstructured data
- Example: `R5`, `R5b`

### Storage Optimized
- Great for storage instensive tasks that require high, sequential read and write access to large data sets on local storage
- Use cases:
	- High frequency online transaction processing (OLTP) systems
	- Relational & NoSQL databases
	- Cache for in-memory databases (for example, Redis)
	- Data warehousing applications
	- Distributed file systems
- Example: `I3`, `I3en`