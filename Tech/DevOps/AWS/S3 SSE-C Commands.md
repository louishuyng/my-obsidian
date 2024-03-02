---
tags: AWS, Devops
---

- Bucket Name: *sec-testing *

### Create Test Local Object
```bash
echo "Test S3 Server Side Encryption with customer provided key SSE-C" > ssec-put.txt
```

### Create own key
```bash
openssl rand 32 -out s3ssec.key
```
### put the object

```bash
aws s3 cp ssec-put.txt s3://sec-testing/ssec-put.txt --sse-c AES256 --sse-c-key fileb://s3ssec.key
```

### get the object
```bash
aws s3 cp s3://sec-testing/ssec-put.txt ssec-get.txt --sse-c AES256 --sse-c-key fileb://s3ssec.key
```

### Check the content of the key
```bash
cat ssec-get.txt
```