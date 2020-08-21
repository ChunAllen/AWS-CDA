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