# Exam Notes Part 5

### To guarantee ordering of a message in SQS FIFO queue the attribute should belong to
* MessageGroupID - is the tag that specifies that a message belongs to a specific message group. Messages that belong to the same message group are always processed one by one, in a strict order relative to the message group (however, messages that belong to different message groups might be processed out of order).

**Other Comparison:**
* MessageDeduplicationID - is the token used for the deduplication of sent messages. If a message with a particular message deduplication ID is sent successfully, any messages sent with the same message deduplication ID are accepted successfully but aren't delivered during the 5-minute deduplication interval.

### To enable EC2 instances to run in multiple docker containers simultaneously
* Docker Multi-Container Platform - allows you to define your software stack and store it in an image that can be downloaded from a remote repository. Use the Multicontainer Docker platform if you need to run multiple containers on each instance. The Multicontainer Docker platform does not include a proxy server. Elastic Beanstalk uses Amazon Elastic Container Service (Amazon ECS) to coordinate container deployments to multi-container Docker environments.

**Other Comparison:**
* Docker Single-Container Platform - allows you to define your software stack and store it in an image that can be downloaded from a remote repository. Use the Single Container Docker platform if you only need to run a **single** Docker container on each instance in your environment. The single container platform includes an Nginx proxy server.

* Custom Platform - provides more advanced customization than a custom image in several ways. A custom platform lets you develop an entirely new platform from scratch, customizing the operating system, additional software, and scripts that Elastic Beanstalk runs on platform instances. 

### Running X-Ray Daemon in Docker Container deployed using Fargate
* Deploy the X-Ray Daemon agent as a sidecar container - In Amazon ECS, create a Docker image that runs the X-Ray daemon, upload it to a Docker image repository, and then deploy it to your Amazon ECS cluster. You can use port mappings and network mode settings in your task definition file to allow your application to communicate with the daemon container.
As we are using AWS Fargate, we do not have control over the underlying EC2 instance and thus we can't deploy the agent on the EC2 instance or run an X-Ray agent container as a daemon set (only available for ECS classic).

* Provide the correct IAM task role to the X-Ray container - For Fargate, we can only provide IAM roles to tasks, which is also the best security practice should we use EC2 instances.

### AWS Lambda functions that are triggered by Amazon API Gateway and would like to perform testing on a low volume of traffic for new API versions.
* Perform Canary Deployment -  total API traffic is separated at random into a production release and a canary release with a preconfigured ratio. Typically, the canary release receives a small percentage of API traffic and the production release takes up the rest. 

### To identify the true IP address of the client in Application Load Balancer
* Look into the `X-Forwarded-For` header in the backend


### X-Forwarded-For vs X-Forwarded-Proto
* X-Forwarded-For - helps you identify the IP address of a client
* X-Forwarded-Proto - helps you identify the protocol (HTTP or HTTPS) that a client used to connect to your load balancer.

### Understanding Bucket Policy of AWS S3
* Bucket policy is an access policy option available for you to grant permission to your Amazon S3 resources. It uses JSON-based access policy language. If you want to configure an existing bucket as a static website that has public access, you must edit block public access settings for that bucket. You may also have to edit your account-level block public access settings.

### How to bundle Lambda function to add dependencies
* Put the function and the dependencies in one folder and zip them together - A deployment package is a ZIP archive that contains your function code and dependencies. You need to create a deployment package if you use the Lambda API to manage functions, or if you need to include libraries and dependencies other than the AWS SDK. You can upload the package directly to Lambda, or you can use an Amazon S3 bucket, and then upload it to Lambda. If the deployment package is larger than 50 MB, you must use Amazon S3. This is the standard way of packaging Lambda functions.

### Exposing HTTP to HTTPS endpoint 
* Open up port 80 & port 443
* Configure EC2 instances to redirect HTTP traffic to HTTPS
* Assign an SSL cert to the Load Balancer

### Which policy should you put to give buckets read access?
* `S3ReadPolicy`

### S3 Policies
* `S3ReadPolicy` - gives read-only permission to objects in S3 bucket
* `S3CrudPolicy` - gives create, read, update, delete permissions to objects in S3 bucket

### What is the maximum number of EC2 instances that can be deployed to process the shards?
* the same number of shards can be deployed for EC2 instance. For e.g. 10 shards = max 10 EC2 instances

### What is the best way to store temporary file for you lambda functions that will be discarded when the function stops running
* Use the local directory `/tmp` - this is 512 MB temporary space you can use for your Lambda functions

### Improve ElastiCache without incurring costs and cache only the most often requested. 
* Use a Lazy Loading strategy with TTL - is a caching strategy that loads data into the cache only when necessary. Whenever your application requests data, it first requests the ElastiCache cache. If the data exists in the cache and is current, ElastiCache returns the data to your application. If the data doesn't exist in the cache or has expired, your application requests the data from your data store. Your datastore then returns the data to your application. In this case, data that is actively requested by users will be cached in ElastiCache, and thanks to the TTL, we can expire that data after a minute to limit the data staleness.

### Collecting X-Ray traces across multiple applications and index traces to search and filter through them efficiently
* Annotations - are simple key-value pairs that are indexed for use with filter expressions. Use annotations to record data that you want to use to group traces in the console, or when calling the GetTraceSummaries API. X-Ray indexes up to `50 annotations per trace`.

### Understanding X-Ray components:
* Metadata -  are key-value pairs with values of any type, including objects and lists, but that is not indexed. Use metadata to record data you want to store in the trace but don't need to use for searching traces.

* Segments - the computing resources running your application logic send data about their work as segments. A segment provides the resource's name, details about the request, and details about the work done.

* Sampling - To ensure efficient tracing and provide a representative sample of the requests that your application serves, the X-Ray SDK applies a sampling algorithm to determine which requests get traced. By default, the X-Ray SDK records the first request each second, and five percent of any additional requests.


### To declare Lambda Function in CloudFormation 
* Upload all the code as a zip to S3 and refer the object in `AWS::Lambda::Function` block
* Write the AWS Lambda code inline in CloudFormation in the `AWS::Lambda::Function` block as long as there are no third-party dependencies

**YAML template for Creating Lambda Function**
```
Type: AWS::Lambda::Function
Properties:
  Code:
    Code
  DeadLetterConfig:
    DeadLetterConfig
  Description: String
  Environment:
    Environment
  FileSystemConfigs:
    - FileSystemConfig
  FunctionName: String
  Handler: String
  KmsKeyArn: String
  Layers:
    - String
  MemorySize: Integer
  ReservedConcurrentExecutions: Integer
  Role: String
  Runtime: String
  Tags:
    - Tag
  Timeout: Integer
  TracingConfig:
    TracingConfig
  VpcConfig:
    VpcConfig
```

### How can you make sure the database can be explored after ELB environment is deleted
* Make a snapshot of the database before it gets deleted

### SNS vs SQS vs Kinesis vs SES
* SNS -  enables message filtering and fanout to a large number of subscribers, including serverless functions, queues, and distributed systems. Additionally, Amazon SNS fans out notifications to end users via mobile push messages, SMS, and email.
* SQS - is a distributed queuing system. Messages are not pushed to receivers. Receivers have to poll SQS to receive messages
* Kinesis - This is used for processing real-time streams meant for big data workloads.
* SES - is an inexpensive way to send and receive emails.

### How to unified X-Ray traces across multiple AWS accounts 
* Create a role in the target unified account and allow roles in each sub-account to assume the role
* Configure the X-Ray daemon to use an IAM instance role
- The X-Ray agent can assume a role to publish data into an account different from the one in which it is running. This enables you to publish data from various components of your application into a central account.

### How to update the ElastiCache data whenever data is written to the database?
* Use a `Write Through Strategy` -  adds data or updates data in the cache whenever data is written to the database.

### What is DynamoDB Accelerator (DAX) 
* is a fully managed, highly available, in-memory cache for Amazon DynamoDB that delivers up to a 10 times performance improvement—from milliseconds to microseconds—even at millions of requests per second.
* It is similar to ElastiCache but for DynamoDB

### How to set repititive tasks asynchronously in Elastic Beanstalk 
* Setup worker environment and a `cron.yaml` file - An environment is a collection of AWS resources running an application version. An environment that pulls tasks from an Amazon Simple Queue Service (Amazon SQS) queue runs in a worker environment tier.

### DynamoDB DAX vs DynamoDB Streams
* DynamoDB DAX - is a fully managed, highly available, in-memory cache for Amazon DynamoDB that delivers up to a 10 times performance improvement—from milliseconds to microseconds—even at millions of requests per second.
- to reduce the read traffic on your AWS DynamoDB table with minimal effort
* DynamoDB Streams - A stream record contains information about a data modification to a single item in a DynamoDB table. 

### What will happened when you version objects in AWS S3?
* Overwriting a file increases its versions
* Deleting a file is a recoverable operation
* Any file that was unversioned before enabling versioning will have the 'null' version 

### Create new service in ECS using AWS CLI in default cluster
```
aws ecs create-service --service-name ecs-simple-service --task-definition ecs-demo --desired-count 10
```
* To create a new service you would use this command which creates a service in your default region called `ecs-simple-service`. The service uses the ecs-demo task definition and it maintains 10 instantiations of that task.

### Which CLI options will you use to paginate the results of an S3 list to only show 100 results per page. 
* 3 options to to return large item list
```
--max-items
--starting-token
--page-size
```
* Full Command
```
aws s3api list-objects --bucket my-bucket --max-items 100 --starting-token eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAxfQ==
```
* By default, the AWS CLI uses a page size of 1000 and retrieves all available items. For example, if you run aws s3api list-objects on an Amazon S3 bucket that contains 3,500 objects, the AWS CLI makes four calls to Amazon S3, handling the service-specific pagination logic for you in the background and returning all 3,500 objects in the final output.

### X-Ray command to ensure that the daemon is correctly discovered on ECS
* `AWS_XRAY_DAEMON_ADDRESS` - Set the host and port of the X-Ray daemon listener. By default, the SDK uses 127.0.0.1:2000 for both trace data (UDP) and sampling (TCP). Use this variable if you have configured the daemon to listen on a different port or if it is running on a different host.

### X-Ray Commands 
* `AWS_XRAY_DAEMON_ADDRESS` - to override the daemon to listen on a different port 
* `AWS_XRAY_TRACING_NAME` - this sets a service name that the SDK uses for segments.
* `AWS_XRAY_CONTEXT_MISSING` - This should be set to LOG_ERROR to avoid throwing exceptions when your instrumented code attempts to record data when no segment is open.
* `AWS_XRAY_DEBUG_MODE` - This should be set to TRUE to configure the SDK to output logs to the console, instead of configuring a logger.

### What is Lambda@Edge
*  a feature of Amazon CloudFront that lets you run code closer to users of your application, which improves performance and reduces latency. With Lambda@Edge, you don't have to provision or manage infrastructure in multiple locations around the world. You pay only for the compute time you consume - there is no charge when your code is not running.

### How can you remove older versions that are not used by Elastic Beanstalk so that new versions can be created for your applications
* Use a Lifecycle Policy - Each time you upload a new version of your application with the Elastic Beanstalk console or the EB CLI, Elastic Beanstalk creates an application version. If you don't delete versions that you no longer use, you will eventually reach the application version limit and be unable to create new versions of that application.

### What must be done to setup HTTPS on Beanstalk?
* Create a `.ebextension` file to configure the Load Balancer -To update your AWS Elastic Beanstalk environment to use HTTPS, you need to configure an HTTPS listener for the load balancer in your environment. Two types of load balancers support an HTTPS listener: Classic Load Balancer and Application Load Balancer. e.g. `.ebextensions/securelistener-alb.config`

### To update exchanges between multiple tables 
* DynamoDB Transactions - You can use DynamoDB transactions to make coordinated all-or-nothing changes to multiple items both within and across tables. Transactions provide atomicity, consistency, isolation, and durability (ACID) in DynamoDB, helping you to maintain data correctness in your applications.

### DynamoDB Transactions, DynamoDB Streams, DynamoDB TTL, DynamoDB Indexes
* DynamoDB Transactions - read from above
* DynamoDB Streams - gives a changelog of changes that happened to your tables and then may even relay these to a Lambda function for further processing.
* DynamoDB TTL -  allows you to expire data based on a timestamp, so this option is not correct.
* DynamoDB Indexes - GSI and LSI are used to allow you to query your tables using different partition/sort keys.

### one-stop dashboard for all the CI/CD
* CodeStar enables you to quickly develop, build, and deploy applications on AWS. AWS CodeStar provides a unified user interface, enabling you to easily manage your software development activities in one place. With AWS CodeStar, you can set up your entire continuous delivery toolchain in minutes, allowing you to start releasing code faster. 

### To avoid potential throttling in DynamoDB
* The GSI is throttling so you need to provision more RCU and WCU to the GSI - the provisioned write capacity for a global secondary index should be equal or greater than the write capacity of the base table since new updates will write to both the base table and global secondary index.

### To invoke AWS Lambda function every hour similar to CRON in a serverless way
* Use CloudWatch Events - You can create a Lambda function and direct CloudWatch Events to execute it on a regular schedule. You can specify a fixed rate (for example, execute a Lambda function every hour or 15 minutes), or you can specify a Cron expression.

### What intrinic function will you use to use the exported value in another stack?
* `!ImportValue` - returns the value of an output exported by another stack. You typically use this function to create cross-stack references.