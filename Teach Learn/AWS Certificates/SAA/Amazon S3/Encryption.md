- There are 4 methods of encrypting objects in S3
	- SSE-S3: encrypts S3 objects using key handled & managed by AWS
	- SSE-KMS: leverage AWS Key Management Service to manage encryption keys
	- SSE-C: when you want to mange your own encryption keys
	- Client Side Encryption
### SSE-S3
- encryption using keys handled and managed by AWS S3
- Object is encrypted server side
- AES-256 encryption type
- Must set header: "x-amz-server-side-encryption": "AES256"
![[Screen Shot 2022-05-15 at 16.02.50.png]]

### SSE-KMS
- encryption using keys handled & managed by KMS
- KMS Advantages: user control + audit trail
-  Object is encrypted server side
- Must set header: "x-amz-server-side-encryption": "aws:kms"
![[Screen Shot 2022-05-15 at 16.05.11.png]]

### SSE-C
- server-side encryption using dat keys fully managed by the customer outside of AWS
- Amazon S3 does not store the encryption key you provide
- HTTPS must be used
- Encryption key must provided in HTTP headers, for every HTTP request made
![[Screen Shot 2022-05-15 at 16.07.46.png]]

### Client Side Encryption
- Client library such as the Amazon S3 Encryption Client
- Client must encrypt data themselves before sending to S3
- Client must decrypt data themselves when retrieving from S3
- Customer fully manages the keys and encryption cycle
![[Screen Shot 2022-05-15 at 16.10.48.png]]

## Encryption in transit (SSL/TLS)
- Amazon S3 exposes:
	- HTTP endpoint: non encrypted
	- HTTPS endpoint: encryption in flight
- Most clients would use the HTTPS endpoint by default
- HTTPS is mandatory for SSE-C
- Encryption in flight is also called SSL/ TLS