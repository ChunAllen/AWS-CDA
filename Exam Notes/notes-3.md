# Exam Notes Part 3

### Reducing cost and can deal with high volume of read traffic
* Setup ElastiCache in front of RDS
**Note:**
  * Setting up RDS Read replicas increases the costs and will not help with reducing the latency when compared to caching solution

### Security principles on Amazon S3
* Bucket Polices
* Access Control List (ACL)
* IAM Roles
**Note:** 
  * S3 doesn't have security groups

### EBS Encryption supports the following:
* a volume restored from an encrypted snapshot, or a copy of an encrypted snapshot is always encrypted
* Encryption by default is region specific. If you enable if for your region, you cannot disable it for individual volumes or snashot in that region

### Highly reliable fully-managed caching layer in front of RDS
* Implement Amazon ElastiCache Redis in Cluster Mode
  -  mode enabled to enhance reliability and availability with little change to your existing workload. Cluster mode comes with the primary benefit of horizontal scaling of your Redis cluster, with almost zero impact on the performance of the cluster.

**Others:**
  * Implement Amazon ElastiCache Memcached -  Redis and Memcached are popular, open-source, in-memory data stores. Although they are both easy to use and offer high performance, there are important differences to consider when choosing an engine. Memcached is designed for simplicity while Redis offers a rich set of features that make it effective for a wide range of use cases. Redis offers snapshots facility, replication, and supports transactions, which Memcached cannot and hence ElastiCache Redis is the right choice for our use case.


### ElastiCache use cases:
* improve latency and throughput for read heavy application workloads
* improve performance of compute intensive workloads

### Hot partition in DynamoDB
* It's not always possible to distribute read and write activity evenly. When data access is imbalanced, a "hot" partition can receive a higher volume of read and write traffic compared to other partitions.
* If one of the partitions receives higher read and write traffic compared to other partitions

### ECS on Fargate vs EC2
* Amazon Elastic Container Service (ECS) on Fargate
* ECS on EC2 - not considered as serverless

### To provide a shared data storage for sessions the can be accessed from any individual web server
* Add an ElastiCache Cluster 


### Your applications can publish metrics to CloudWatch with 1-second resolution. You can watch the metrics scroll across your screen seconds after they are published and you can set up high-resolution CloudWatch Alarms that evaluate as frequently as every 10 seconds
* Create a high resolution custom metric and push data using script triggered every 10 seconds 


### Following options for protecting data at rest in Amazon S3:
* Server-Side Encryption – Request Amazon S3 to encrypt your object before saving it on disks in its data centers and then decrypt it when you download the objects.

* Client-Side Encryption – Encrypt data client-side and upload the encrypted data to Amazon S3. In this case, you manage the encryption process, the encryption keys, and related tools.

* Server-side encryption with AWS KMS (SSE-KMS), you can use the default AWS managed CMK, or you can specify a customer-managed CMK that you have already created.
Creating your own customer-managed CMK gives you more flexibility and control over the CMK.

### Load Balancer with Sticky Sessions enabled
* Sticky sessions are a mechanism to route requests to the same target in a target group. This is useful for servers that maintain state information to provide a continuous experience to clients. To use sticky sessions, the clients must support cookies.

### Helps for disaster recovery in RDS
* Enable automated backup feature that create backups in a single AWS region. (There's no backup for multiple regions)
* Enable Multi AZ Deployments 

### Use case where Lambda function will send a message to Dead Letter Queue
* If event fails all processing attempts in Lambda Functions or SQS
* If lambda function invocations is asynchronous

### Blue/Green Deployment
is used to update your applications **while minimizing interruptions caused by the changes of a new application version.** CodeDeploy **provisions your new application version alongside the old version** before rerouting your production traffic. 

### Reducing the number of calls made to endpoint and improve latency in API Gateway
* Enable API Gateway Caching

### To ensure efficient tracing and provide a representative sample of the requests that your application serves,
* Enable X-Ray Sampling

### CloudTrail components:
* Member can accounts will be able to see Organization trail but cannot modify or delete it
* By default, CloudTrail tracks only bucket-level actions. To track object-level actions, you need to enable Amazon S3 data events

### SQS retention period min and max
* 4 days minimum, 14 days maximum

### AWS Step Functions state machine vs Step Functions activities
* **Step Functions State Machine** - Step Functions are based on the concepts of tasks and state machines. You define state machines using the JSON-based Amazon States Language. A state machine is defined by the states it contains and the relationships between them. States are elements in your state machine. Individual states can make decisions based on their input, perform actions, and pass output to other states.

* **Step Functions activities** - In AWS Step Functions, activities are a way to associate code running somewhere (known as an activity worker) with a specific task in a state machine. When a Step Function reaches an activity task state, the workflow waits for an activity worker to poll for a task. For example, an activity worker can be an application running on an Amazon EC2 instance or an AWS Lambda function.

### An Amazon FIFO SQS queue might have been configured as the destination. 
* Only standard SQS queues are supported for Amazon S3 event notification destination - Currently, only **Standard SQS queue** is allowed as an Amazon S3 event notification destination and FIFO SQS queues are not allowed.

### LSI vs GSI DynamoDB
* LSI - An index that has the same partition key as the base table, but a different sort key. A local secondary index is "local" in the sense that every partition of a local secondary index is scoped to a base table partition that has the same partition key value.

* GSI - An index with a partition key and a sort key that can be different from those on the base table. A global secondary index is considered "global" because queries on the index can span all of the data in the base table, across all partitions. A global secondary index is stored in its own partition space away from the base table and scales separately from the base table.


### S3 event notification 
* If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent. If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent.

### SQS Extended Client 
* This is especially useful for storing and consuming messages up to 2 GB. Unless your application requires repeatedly creating queues and leaving them inactive or storing large amounts of data in your queues, consider using Amazon S3 for storing your data.

### Incorrect statement about S3 data consistency model
* S3 supports object locking for concurrent updates


### To enable detailed monitoring of the EC2 instances using AWS CLI
```
aws ec2 monitor-instances --instance-ids i-1234567890abcdef0
```

### Create a custom metric in CloudWatch and make your instances send data to it using PutMetricData. Then, create an alarm based on this metric
You can create a custom CloudWatch metric for your EC2 Linux instance statistics by creating a script through the AWS Command Line Interface (AWS CLI). Then, you can monitor that metric by pushing it to CloudWatch.

### is a software package that, when installed and configured on an instance, makes it possible for that instance to be used in CodeDeploy deployments.
* CodeDeploy Agent

### is a web service that monitors your AWS resources and the applications you run on AWS. 
* CloudWatch Events

### SQS Short Polling vs Long Polling
* Short Polling - This is the default polling of SQS 
* Long Polling - makes it inexpensive to retrieve messages from your Amazon SQS queue as soon as the messages are available