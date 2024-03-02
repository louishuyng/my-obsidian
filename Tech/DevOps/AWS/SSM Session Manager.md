---
tags: Devops, AWS
---
• Allows you to start a secure shell on your EC2 and on- premises servers 
• Access through AWS Console, AWS CLI, or Session Manager SDK 
• Does not need SSH access, bastion hosts, or SSH keys

### Control through IAM Permissions
- Control which users/groups can access Session Manager and which instances 
- Use tags to restrict access to only specific EC2 instances
- Access SSM + write to S3 + write to CloudWatch
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ssm:StartSession",
			"Resource": "arn: aws: ec2:us -east -1:123456789012:instance/*",
			"Condition": {
				"StringLike": {
					"ssm: resourceTag/Environment": ["Dev"]
				}
			}
		}
	]
}
```

### Comparison with SSH
![[Pasted image 20230709172937.png]]