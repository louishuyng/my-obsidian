---
tags: AWS, DevOps
---
## What is the AWS CLI?

AWS CLI stands for Amazon Web Services Command Line Interface. When managing your AWS services there are a few options as far as tools go. Two of the most common options are using the AWS Console, or AWS CLI. The AWS Console is a web interface that you log into to manage your AWS services. In contrast to the AWS Console is AWS CLI. It is a great tool to manage AWS resources across different accounts, regions, and environments from the command line. It allows you to control services manually or create automation with scripts.

If you haven't installed AWS CLI yet start at the [Installing the AWS CLI Guide from Amazon](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html).

**Pro-tip 1 - use the command-completion feature.**

We think the best cheatsheet you can have for AWS CLI is the command-completion feature. It allows you to use the Tab key to complete a partially entered command. It will either complete your command or display a list of suggested commands. It isn't always automatically installed, so you'll need to configure it manually. Here is the [AWS guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-completion.html) to get it up and running.

**Pro-tip 2 - use the help command.**

When you need a little extra help just lean on the AWS CLI help command to get detailed documentation on what is available. To use this command you just append help at the end of a command name. For example, if you do 'aws help' it will show the general AWS CLI options and list all the services. If you need to see what all the available commands for AWS EC2 specifically, you would type 'aws ec2 help.' It will become a huge aid to you in becoming an AWS CLI pro.

**Pro-tip 3 - use jq.**

This cheatsheet utilizes jq, a lightweight and flexible command-line JSON processor. We highly recommend using it for AWS CLI. You can find more information on it at the [Github repository for it](https://stedolan.github.io/jq/).

## Config

Create profiles

```json
aws configure --profile profilename
```

Output format

```json
aws configure output format {json, yaml, yaml-stream, text, table}
```

Specify your AWS Region

```json
aws configure region (region-name)
```

## API Gateway

List API Gateway IDs and Names

```json
aws apigateway get-rest-apis | jq -r ‘.items[ ] | .id+” “+.name’
```

List API Gateway keys

```json
aws apigateway get-api-keys | jq -r ‘.items[ ] | .id+” “+.name’
```

List API Gateway domain names

```json
aws apigateway get-domain-names | jq -r ‘.items[ ] | .domainName+” “+.regionalDomainName’
```

List resources for API Gateway

```json
aws apigateway get-resources --rest-api-id ee86b4cde | jq -r ‘.items[ ] | .id+” “+.path’
```

Find Lambda for API Gateway resource

```json
aws apigateway get-integration --rest-api-id (id) --resource-id (resource id) --http-method GET | jq -r ‘.uri’
```

## Amplify

List Amplify apps and source repository

```json
aws amplify list-apps | jq -r ‘.apps[ ] | .name+” “+.defaultDomain+”
```

## CloudFront

List CloudFront distributions and origins

```json
aws cloudfront list-distributions | jq -r ‘.DistributionList.Items[ ] | .DomainName+” “+.Origins.Items[0].DomainName’
```

Create a new invalidation

```json
aws cloudfront create-invalidation [distribution-id]
```

## CloudWatch

List information about an alarm

```json
aws cloudwatch describe-alarms | jq -r ‘.MetricAlarms[ ] | .AlarmName+” “+.Namespace+” “+.StateValue’
```

Delete an alarm or alarms (you can delete up to 100 at a time)

```json
aws cloudwatch delete-alarms --alarm-names (alarmnames)
```

## Cognito

List user pool IDs and names

```json
aws cognito-idp list-user-pools --max-results 60 | jq -r ‘.UserPools[ ] | .Id+” “+.Name’
```

List phone and email of all users

```json
aws cognito-idp list-users --user-pool-id (resource) | jq -r ‘.Users[ ].Attributes | from_entries | .sub + “ “ + .phone_number + “ “ + .email’
```

## DynamoDB

List DynamoDB tables

```json
aws dynamodb list-tables | jq -r .TableNames [ ]
```

Get all items from a table

```json
aws dynamodb scan --table-name events
```

Get item count from a table

```json
aws dynamodb scan --table-name events --select count | jq .ScannedCount
```

Get item using key

```json
aws dynamodb get-item --table-name events --key ‘{“email””"email@example.com”}}’
```

Get specific fields from an item

```json
aws dynamodb get-item --table-name events --key ‘{“email””"email@example.com"}}’ --attributes-to-get event_type
```

Delete item using key

```json
aws dynamodb delete-item --table-name events --key ‘{“email””email@domain.com”}}’
```

## EBS

Complete a Snapshot

```json
aws ebs complete-snapshot (snapshot-id)
```

Start a Snapshot

```json
aws ebs start-snapshot --volume-size (value)
```

Get a Snapshot block

```json
aws ebs get-snapshot-block
```

```json
--snapshot-id (value)
```

```json
--block-index (value)
```

```json
--block-token (value)
```

## EC2

List Instance ID, Type and Name

```json
aws ec2 describe-instances | jq -r '.Reservations[].Instances[]|.InstanceId+" "+.InstanceType+" "+(.Tags[] | select(.Key == "Name").Value)'
```

List Instances with public IP address and Name

```json
aws ec2 describe-instances --query 'Reservations[*].Instances[?not_null(PublicIpAddress)]' | jq -r '.[][]|.PublicIpAddress+" "+(.Tags[]|select(.Key=="Name").Value)'
```

List VPCs and CIDR IP Block

```json
aws ec2 describe-vpcs | jq -r '.Vpcs[]|.VpcId+" "+(.Tags[]|select(.Key=="Name").Value)+" "+.CidrBlock'
```

List Subnets for a VPC

```json
aws ec2 describe-subnets --filter Name=vpc-id,Values=vpc-0d1c1cf4e980ac593 | jq -r '.Subnets[]|.SubnetId+" "+.CidrBlock+" "+(.Tags[]|select(.Key=="Name").Value)'
```

List Security Groups

```json
aws ec2 describe-security-groups | jq -r '.SecurityGroups[]|.GroupId+" "+.GroupName'
```

Print Security Groups for an Instance

```json
aws ec2 describe-instances --instance-ids i-0dae5d4daa47fe4a2 | jq -r '.Reservations[].Instances[].SecurityGroups[]|.GroupId+" "+.GroupName'
```

Edit Security Groups of an Instance

```json
aws ec2 modify-instance-attribute --instance-id i-0dae5d4daa47fe4a2 --groups sg-02a63c67684d8deed sg-0dae5d4daa47fe4a2
```

Print Security Group Rules as FromAddress and ToPort

```json
aws ec2 describe-security-groups --group-ids sg-02a63c67684d8deed | jq -r '.SecurityGroups[].IpPermissions[]|. as $parent|(.IpRanges[].CidrIp+" "+($parent.ToPort|tostring))'
```

Add Rule to Security Group

```json
aws ec2 authorize-security-group-ingress --group-id sg-02a63c67684d8deed --protocol tcp --port 443 --cidr 35.0.0.1
```

Delete Rule from Security Group

```json
aws ec2 revoke-security-group-ingress --group-id sg-02a63c67684d8deed --protocol tcp --port 443 --cidr 35.0.0.1
```

Edit Rules of Security Group

```json
aws ec2 update-security-group-rule-descriptions-ingress --group-id sg-02a63c67684d8deed --ip-permissions 'ToPort=443,IpProtocol=tcp,IpRanges=[{CidrIp=202.171.186.133/32,Description=Home}]'
```

Delete Security Group

```json
aws ec2 delete-security-group --group-id sg-02a63c67684d8deed
```

## ECS

Create an ECS cluster

```json
aws ecs create-cluster --cluster-name=NAME --generate-cli-skeleton
```

Create an ECS service

```json
aws ecs create-service
```

## EKS

Create a cluster

```json
aws eks create-cluster --name (cluster name)
```

Delete a cluster

```json
aws eks delete-cluster --name (cluster name)
```

List descriptive information about a cluster

```json
aws eks describe-cluster --name (cluster name)
```

List clusters in your default region

```json
aws eks list-clusters
```

Tag a resource

```json
aws eks tag-resource --resource-arn (resource_ARN) --tags (tags)
```

Untag a resource

```json
aws eks untag-resource --resource-arn (resource_ARN) --tag-keys (tag-key)
```

## ElastiCache

Get information about a specific cache cluster

```json
aws elasticache describe-cache-clusters | jq -r ‘.CacheClusters[ ] | .CacheNodeType+” “+.CacheClusterId’
```

List ElastiCache replication groups

```json
aws elasticache describe-replication-groups | jq -r ‘.ReplicationGroups [ ] | .ReplicationGroupId+” “+.NodeGroups[ ].PrimaryEndpoint.Address’
```

List ElastiCache snapshots

```json
aws elasticache describe-snapshots | jq -r ‘.Snapshots[ ] | .SnapshotName’
```

Create ElastiCache snapshot

```json
aws elasticache create-snapshot --snapshot-name backend-login-hk-snap-1 --replication-group-id backend-login-hk --cache-cluster-id backend-login-hk
```

Delete ElastiCache snapshot

```json
aws elasticache delete-snapshot --snapshot-name login-snap-1
```

Scale up/down ElastiCache replica

```json
aws elasticache increase-replica-count --replication-group-id backend-login --apply-immediately
```

```json
aws elasticache decrease-replica-count --replication-group-id backend-login --apply-immediately
```

## ELB

List ELB Hostnames

```json
aws elbv2 describe-load-balancers --query ‘LoadBalancers[*].DNSName’ | jq -r ‘to_entries[ ] | .value’
```

List ELB ARNs

```json
aws elbv2 describe-load-balancers | jq -r ‘.LoadBalancers[ ] | .LoadBalancerArn’
```

List of ELB target group ARNs

```json
aws elbv2 describe-target-groups | jq -r ‘.TargetGroups[ ] | .TargetGroupArn’
```

Find instances for a target group

```json
aws elbv2 describe-target-health --target-group-arn arn:aws:elasticloadbalancing:ap-northwest-1:20394823094:targetgroup/wordpress-ph/203942b32a23 | jq -r ‘.TargetHealthDescriptions[ ] | .Target.Id’
```

## IAM Group

List groups

```json
aws iam list-groups | jq -r .Groups[ ].GroupName
```

Add/Delete groups

```json
aws iam create-group --group-name (groupName)
```

List policies and ARNs

```json
aws iam list-policies | jq -r ‘.Policies[ ]|.PolicyName+” “+.Arn’
```

```json
aws iam list-policies --scope AWS | jq -r ‘.Policies[ ]|.PolicyName+” “+.Arn’
```

```json
aws iam list-policies --scope Local | jq -r ‘.Policies[ ]|.PolicyName+” “+.Arn’
```

List user/group/roles for a policy

```json
aws iam list-entities-for-policy --policy-arn arn:aws:iam:2308345:policy/example-ReadOnly
```

List policies for a group

```json
aws iam list-attached-group-policies --group-name (groupname)
```

Add policy to a group

```json
aws iam attach-group-policy --group-name (groupname) --policy-arn arn:aws:iam::aws:policy/exampleReadOnlyAccess
```

Add user to a group

```json
aws iam add-user-to-group --group-name (groupname) --user-name (username)
```

Remove user from a group

```json
aws iam remove-user-from-group --group-name (groupname) --user-name (username)
```

List users in a group

```json
aws iam get-group --group-name (groupname)
```

List groups for a user

```json
aws iam list-groups-for-user --user-name (username)
```

Attach/detach policy to a group

```json
aws iam attach-group-policy --group-name (groupname) --policy-arn arn:aws:iam::aws:policy/DynamoDBFullAccess
```

```json
aws iam detach-group-policy --group-name (groupname) --policy-arn arn:aws:iam::aws:policy/DynamoDBFullAccess
```

## IAM User

List userId and UserName

```json
aws iam list-users | jq -r ‘.Users[ ]|.UserId+” “+.UserName’
```

Get single user

```json
aws iam get-user --user-name (username)
```

Add user

```json
aws iam create-user --user-name (username)
```

Delete user

```json
aws iam delete-user --user-name (username)
```

List access keys for user

```json
aws iam list-access-keys --user-name (username) | jq -r .AccessKeyMetadata[ ].AccessKeyId
```

Delete access key for user

```json
aws iam delete-access-key --user-name (username) --access-key-id (accessKeyID)
```

Activate/deactivate access key for user

```json
aws iam update-access-key --status Active --user-name (username) --access-key-id (access key)
```

```json
aws iam update-access-key --status Inactive --user-name (username) --access-key-id (access key)
```

Generate new access key for user

```json
aws iam create-access-key --user-name (username) | jq -r ‘.AccessKey | .AccessKeyId+” “+.SecretAccessKey’
```

## Lambda

List Lambda functions, runtime, and memory

```json
aws lambda list-functions | jq -r ‘.Functions[ ] | .FunctionName+” “+.Runtime+” “+(.MemorySize|tostring)’
```

List Lambda layers

```json
aws lambda list-layers | jq -r ‘.Layers[ ] | .LayerName’
```

List source event for Lambda

```json
aws lambda list-event-source-mappings | jq -r ‘.EventSourceMappings[ ] | .FunctionArn+” “+.EventSourceArn’
```

Download Lambda code

```json
aws lambda get-function --function-name DynamoToSQS | jq -r .Code.Location
```

## RDS

List DB clusters

```json
aws rds describe-db-clusters | jq -r ‘.DBClusters[ ] | .DBClusterIdentifier+” “+.Endpoint’
```

List DB instances

```json
aws rds describe-db-instances | jq -r ‘.DBInstances[ ] | .DBInstanceIdentifier+” “+.DBInstanceClass+” “+.Endpoint.Address’
```

Take DB Instance Snapshot

```json
aws rds create-db-snapshot --db-snapshot-identifier snapshot-1 --db-instance-identifier dev-1
```

```json
aws rds describe-db-snapshots --db-snapshot-identifier snapshot-1 --db-instance-identifier general
```

Take DB cluster snapshot

```json
aws rds create-db-cluster-snapshot --db-cluster-snapshot-identifier
```

## Route53

Create hosted zone

```json
aws route53 create-hosted-zone --name exampledomain.com
```

Delete hosted zone

```json
aws route53 delete-hosted-zone --id example
```

Get hosted zone

```json
aws route53 get-hosted-zone --id example
```

List hosted zones

```json
aws route53 list-hosted-zones
```

Create a record set

To do this you’ll first need to create a JSON file with a list of change items in the body and use the CREATE action. For example the JSON file would look like this.

```json
{
     "Comment": "CREATE/DELETE/UPSERT a record",
     "Changes": [{
     "Action": "CREATE",
          "ResourceRecordSet":{
               "Name": "a.example.com",
               "Type": "A",
               "TTL": 300,
          "ResourceRecords":[{"Value":"4.4.4.4"}]
}}]
}
```

Once you have a JSON file with the correct information like above you will be able to enter the command

```json
aws route53 change-resource-record-sets --hosted-zone-id (zone-id) --change-batch file://exampleabove.json
```

Update a record set

To do this you’ll first need to create a JSON file with a list of change items in the body and use the UPSERT action. This will either create a new record set with the specified value, or updates a record set if it already exists. For example the JSON file would look like this.

```json
{
     "Comment": "CREATE/DELETE/UPSERT a record",
     "Changes": [{
     "Action": "UPSERT",
          "ResourceRecordSet":{
               "Name": "a.example.com",
               "Type": "A",
               "TTL": 300,
          "ResourceRecords": [{"Value":"4.4.4.4"}]
}}]
}
```

Once you have a JSON file with the correct information like above you will be able to enter the command

```json
aws route53 change-resource-record-sets --hosted-zone-id (zone-id) --change-batch file://exampleabove.json
```

Delete a record set

To do this you’ll first need to create a JSON file with a list of the record set values you want to delete in the body and use the DELETE action. For example the JSON file would look like this.

```json
{
     "Comment": "CREATE/DELETE/UPSERT a record",
     "Changes": [{
     "Action": "DELETE",
          "ResourceRecordSet": {
               "Name": "a.example.com",
               "Type": "A",
               "TTL": 300,
          "ResourceRecords": [{"Value":"4.4.4.4"}]
}}]
}
```

Once you have a JSON file with the correct information like above you will be able to enter the following command.

```json
aws route53 change-resource-record-sets --hosted-zone-id (zone-id) --change-batch file://exampleabove.json
```

## S3

List Buckets

```json
aws s3 ls
```

List files in a Bucket

```json
aws s3 ls s3://mybucket
```

Create Bucket

```json

aws s3 mb s3://bucket-name
make_bucket: bucket-name
```

Delete Bucket

```json
aws s3 rb s3://bucket-name --force
```

Download S3 object to local

```json
aws s3 cp s3://bucket-name
download: ./backup.tar from s3://bucket-name/backup.tar
```

Upload local file as S3 object

```json
aws s3 cp backup.tar s3://bucket-name
upload: ./backup.tar to s3://bucket-name/backup.tar
```

Delete S3 object

```json
aws s3 rm s3://bucket-name/secret-file.gz .
delete: s3://bucket-name/secret-file.gz
```

Download bucket to local

```json
aws s3 sync s3://bucket-name/ /media/pasport-ultra/backup
```

Upload local directory to bucket

```json

aws s3 sync (directory) s3://bucket-name/
```

Share S3 object without public access

```json

aws s3 presign s3://bucket-name/file-name --expires-in (time value)
https://bucket-name.s3.amazonaws.com/file-name.pdf?AWSAccessKeyId=(key)&Expires=(value)&Signature=(value)
```

## SNS

List SNS topics

```json

aws sns list-topics | jq -r ‘.Topics[ ] | .TopicArn’
```

List SNS topic and related subscriptions

```json

aws sns list-subscriptions | jq -r ‘.Subscriptions[ ] | .TopicArn+” “+.Protocol+” “+.Endpoint’
```

Publish to SNS topic

```json

aws sns publish --topic-arn arn:aws:sns:ap-southeast-1:232398:backend-api-monitoring
```

## SQS

List queues

```json
aws sqs list-queues | jq -r ‘.QueueUrls[ ]’
```

Create queue

```json
aws sqs create-queue --queue-name public-events.fifo | jq -r .queueURL
```

Send message

```json
aws sqs send-message --queue-url (url) --message-body (message)
```

Receive message

```json
aws sqs receive-message --queue-url (url) | jq -r ‘.Messages[ ] | .Body’
```

Delete message

```json
aws sqs delete-message --queue url (url) --receipt-handle (receipt handle)
```

Purge queue

```json
aws sqs purge-queue --queue-url (url)
```

Delete queue

```json
aws sqs delete-queue --queue-url (url)
```