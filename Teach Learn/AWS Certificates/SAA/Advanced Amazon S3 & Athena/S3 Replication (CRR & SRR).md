- Must enable versioning in  source and destination
- Cross Region Replication (CRR)
- Same Region Replication (SRR)
- Buckets can be in different accounts
- Copying is asynchronous
- Must give proper IAM permissions to S3
![[Screen Shot 2022-05-21 at 21.20.12.png]]

- **CRR - Use cases:** compliance, lower latency access, replication across accounts
- **SRR - Use cases:** log aggregation, live replication between production and test accounts

### Notes
- After activating, only new objects are replicated
- Optionally, you can replicate existing objects using **S3 Batch Replication**
	- Replicates existing objects and objects that failed replication
- For DELETE operations:
	- Can replicate delete markers from source to target (optional setting)
	- Deletions with a version ID are not replicated (to avoid malicious deletes)
- There is no 'chaning of replication'
	- If bucket I has replication bucket 2, which has replication into bucket 3
	- Then objects created in bucket I are not replicated to bucket 3