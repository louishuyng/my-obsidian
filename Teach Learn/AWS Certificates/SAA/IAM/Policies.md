### Structure
Consist of:
- Version: policy language version
- Id : optional
- Statement: one or more individual statements
### Statement
Consist of:
- Sid (Optional)
- Effect (Allow / Deny)
- Principal: account/user/role to which this policy applied to
- Action
- Resource list
- Conditional (Optional)

#### Example: 

```json
{
  "Version": "2012-10-17",
  "Id": "default",
  "Statement": [
    {
      "Sid": "s3-event-cezary_for_lambda-s3",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "lambda:InvokeFunction",
      "Resource": "arn:aws:lambda:eu-west-1:1234567890:function:lambda-s3",
      "Condition": {
        "StringEquals": {
          "AWS:SourceAccount": "1234567890:"
        },
        "ArnLike": {
          "AWS:SourceArn": "arn:aws:s3:::test-bucket-cezary"
        }
      }
    }
  ]
}
```