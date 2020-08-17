# Exam Notes Part 2

### CloudFormation supported parameter types
```
String – A literal string
Number – An integer or float
List<Number> – An array of integers or floats
CommaDelimitedList – An array of literal strings that are separated by commas
AWS::EC2::KeyPair::KeyName – An Amazon EC2 key pair name
AWS::EC2::SecurityGroup::Id – A security group ID
AWS::EC2::Subnet::Id – A subnet ID
AWS::EC2::VPC::Id – A VPC ID
List<AWS::EC2::VPC::Id> – An array of VPC IDs
List<AWS::EC2::SecurityGroup::Id> – An array of security group IDs
List<AWS::EC2::Subnet::Id> – An array of subnet IDs
```

### To improve performance of Lambda function without chaning code
* Increase RAM assigned to your Lambda function

### To analyze the patterns for the clients IP
* Use `X-Forwarded-For`

### A development team has configured their Amazon EC2 instances for Auto Scaling. A Developer during routine checks has realized that only basic monitoring is active, as opposed to detailed monitoring. Which of the following represents the best root-cause behind the issue?
* AWS Management Console has been used to create launch configuration 
* Creating launch template launches basic monitoring by default

### The development team wants to postpone the delivery of new messages to the queue for a few seconds.
* Use delay queues to postpone the delivery of new messages to the queue for few seconds

### The bucket size has grown substantially after starting access logging. What might be the cause
* S3 access logging is pointing to the same bucket and is responsible for the substantial growth of the bucket

### An Auto Scaling group has a maximum capacity of 3, a current capacity of 2, and a scaling policy that adds 3 instances. When executing this scaling policy, what is the expected outcome?
* Amazon EC2 Auto Scaling adds only 1 instance to the group, because the maximum capacity is 3 and the current capacity is 2


### To test if the IAM user can trigger an action 
* Use AWS CLI `--dry-run` option

### Load Balancer types
* Check out https://github.com/ChunAllen/AWS-CDA/blob/5854178216125b96785bd1297568463079b7e033/Section-4%20%20ELB%20%26%20ASG/4-1%20Load%20Balancer.md#types-of-load-balancer

### What is `ProvisionedThroughputException`
* This causes the Kinesis to went beyond the limits
**To Solve This**
* Increase number of Shards within your data streams to provide enough capacity
* Configure the data producer to retry with an exponential backoff

### If you have multiple consumers retrieving data from a stream in parallel in Kinesis
* Use Kinesis enhanced fan-out 

### If you want to upload more than 5GB of file in S3
* Use multi-part upload for larger files 
* The maximum size to upload per file is 5GB


### If you would like to place a configuration mechanism on Elastic Beanstalk (EBS)
* Include config files in `.ebextensions/` at the root of your source code

### EBS Volume Types
* Checkout https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html

* 5.3 TiB - General Purpose SSD (GP2) ) volumes offer cost-effective storage that is ideal for a broad range of workloads.

* 10.6 TiB - Provisioned IOPS SSD (io1)
* 16 TiB - Throughput Optimized HDD (st1)
* 2.7 TiB - Cold HDD (sc1)

### CodeDeploy Deployment hook to verify the success of deployment 
* ValidateService
* Other AppSpec Hooks:
  * Checkout https://github.com/ChunAllen/AWS-CDA/blob/6be42c4a23740e6444236a8309fa0148b9cc09d0/Section-15%20AWS%20CI%20%26%20CD/15-4%20CodeDeploy.md#codedeploy-appspec-hooks

### To monitor the logs of AWS Services in S3
* Enable S3 and CloudWatch Logs integration

### These are the actions that Amazon EC2 can be configured when an interuption occurs on an instance:
* Hibernate
* Stop
* Terminate


### Designed for high availability and scalability for sending and receiving SMS
* Api Gateway + Lambda
* ALB + ECS


### SQS messages that aren't processed will end up on 
* Dead Letter Queue


### Eventually Consistent vs Strongly Consistent in DynamoDB
* Eventually Consistent - When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data.

* Strongly Consistent - When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful.

Note: By Default, DynamoDB uses Eventually Consistent, if you set `ConsistentRead` to true it will become Strongly Consistent and only applied to Read Operations

### What's the maxiumum number of messages that can be stored in an SQS queue
* no limit

### Stage variables vs Lambda Aliases
* Stage variables are name-value pairs that you can define as configuration attributes associated with a deployment stage of an API. They act like environment variables and can be used in your API setup and mapping templates. 

* Lambda alias is like a pointer to a specific Lambda function version. Users can access the function version using the alias ARN.

### Here is the correct way of reusing SSH keys in your AWS Regions:
* Generate a public SSH key (.pub) file from the private SSH key (.pem) file.
* Set the AWS Region you wish to import to.
* Import the public SSH key into the new Region.

### Maximum data size supported by KMS 
* 4KB

### Amazon ElastiCache with Amazon S3 as backup
 Amazon ElastiCache is a fully managed in-memory data store, compatible with Redis or Memcached. ElastiCache is NOT a supported destination for Amazon Kinesis Data Firehose.


### FilterExpression vs ProjectionExpression in DynamoDB

* Projection expression - specifies the attributes you want to be shown from the query results 
```
  aws dynamodb get-item \
    --table-name ProductCatalog \
    --key file://key.json \
    --projection-expression "Description, RelatedItems[0], ProductReviews.FiveStar"
```

* FilterExpression - determines which items within the Query results should be returned to you. All of the other results are discarded. A filter expression is applied after Query finishes, but before the results are returned. 


### Appspec.yml vs BuildSpec.yml
* buildspec.yml - This is a file used by AWS CodeBuild to run a build
* appspec.yml - This is a file used by AWS CodeDeploy to execute deployment 


### Zonal vs Regional Reserved Instance
* Zonal Reserved Instance - provides a capacity reservation in the specified Availability Zone. Capacity Reservations enable you to reserve capacity for your Amazon EC2 instances in a specific Availability Zone for any duration. 

* Regional Reserved Instance - when you purchase a reserved instance for a region

**Note:** Regional Reserved Instance does not provide a capacity reservation. 


### Things needed to establish connection between EC2 and internet thru internet gateway
* The route table in the instance's subnet should have route to internet gateway
* The network ACL's associated with the subnet must have rules to allow inbound and outbound traffic

### How to handle `ProvisionedThroughputExceededException` in Kinesis Data Streams while keeping costs at a minimum
* Batch Messages

### CloudFormation Intrinsic Functions
* `!GetAtt` - The Fn::GetAtt intrinsic function returns the value of an attribute from a resource in the template.

* `!Sub` - The intrinsic function Fn::Sub substitutes variables in an input string with values that you specify. In your templates, you can use this function to construct commands or outputs that include values that aren't available until you create or update a stack.

* `!Ref` - The intrinsic function Ref returns the value of the specified parameter or resource.

* `!FindInMap` - The intrinsic function Fn::FindInMap returns the value corresponding to keys in a two-level map that is declared in the Mappings section

### Reserved vs Provisioned Concurrency in Lambda
* Reserved Concurrency - gives a certain limit of concurrency. This will be limited to max `N` number of concurrency
* Provisioned Concurrency - increase provisioned concurrency in anticipation of peak traffic. To increase provisioned concurrency automatically as needed, use the Application Auto Scaling API to register a target and create a scaling policy.

### Facts about Amazon EC2 Auto Scaling 
* works with both Application and Network Load balancers
* cannot add volume to an existing instance if the existing volume is reaching the capacity

### AWS Database Engines with IAM Database Authentication
* RDS PostgreSQL
* RDS MySQL