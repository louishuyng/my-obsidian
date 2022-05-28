-  For audit purpose, you may want to log all access to S3 buckets
- Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket
- That data can be analyzed using data analysis tools
![[Screen Shot 2022-05-21 at 20.58.44.png]]
### Warning
- Do not set your logging bucket to be the monitored bucket
- It will create a logging loop, and your bucket will grow in size exponentially
![[Screen Shot 2022-05-21 at 21.01.07.png]]