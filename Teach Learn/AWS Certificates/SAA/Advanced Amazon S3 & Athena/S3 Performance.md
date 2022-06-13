### Baseline Performance
- Amazon S3 applicatautomatically scales to high request rates, latency 100-200ms
- Your application can archieve at least 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix in a bucket
- There are no limits to the number of prefixes in a blucket
- Example (object path => prefix):
	- bucket/folder1/sub1/file => /folder1/sub1/
	- bucket/folder1/sub2/file => /folder1/sub2/
	- bukcet/1/file => /1/
- If you sread reads across all four prefixes, you can archive 22,000 requests per second for GET and HEAD


### KMS Limitation
- If you use SSE-MKS, you may be impacted by the KMS limits
- When you upload, it calls GenerateDataKey KMS API
- When you download, it calls the Decrypt KMS API
- Count towards the KMS quota per second (5500, 10000, 3000 req/s based on region)
- You can request a quota increase using the Service Quotas Console
![[Screen Shot 2022-06-06 at 11.22.53.png]]

### Performance
- Multi-Part upload:
	- recommended for files > 100 MB, must use for files > 5GB
	- Can help parallelize uploads (speed up transfers)
	![[Screen Shot 2022-06-06 at 11.26.34.png]]
- S3 Transfer Acceleration
	- INcrease transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
	- Compatible with multi-part upload
	![[Screen Shot 2022-06-06 at 11.28.26.png]]

### S3 Byte-Range Fetches
- Parallelize GETs by requesting specific byte ranges
- Better resilience in case of failures

Can be used to speed up downloads 
![[Screen Shot 2022-06-06 at 11.30.03.png]]
Can be used to retrieve only partial data (for example the head of a file)
![[Screen Shot 2022-06-06 at 11.30.41.png]]