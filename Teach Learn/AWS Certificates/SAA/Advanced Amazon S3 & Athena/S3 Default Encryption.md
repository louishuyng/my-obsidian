- One way to "force encryption" is to use a bucket policy and refus API call to PUT an S3 object without encryption headers:
![[Screen Shot 2022-05-21 at 20.29.03.png]]
- Another way is to use the "default encryption" option in S3
- Note: Bucket Policies are evaluated before "default encryption"