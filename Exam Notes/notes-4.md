# Exam Notes Part 4

### Alternative way for distributing traffic to a web application aside from Route53
* Elastic Load Balancer (ELB)

### Can improve the performance of DynamoDB scan operations especially if indexes exists 
* Parallel Scans - The larger the table or index being scanned, the more time the Scan takes to complete. To address these issues, the Scan operation can logically divide a table or secondary index into multiple segments, with multiple application workers scanning the segments in parallel.

### Understanding Kinesis Data Stream 
* Amazon Kinesis Data Streams enables you to build custom applications that process or analyze streaming data for specialized needs.

A Kinesis data stream is a set of shards. A shard is a uniquely identified sequence of data records in a stream. A stream is composed of one or more shards, each of which provides a fixed unit of capacity.

### CloudWatch Event Concepts
* Events - indicates a change in your AWS environment. AWS resources can generate events when their state changes. For example, Amazon EC2 generates an event when the state of an EC2 instance changes from pending to running, and Amazon EC2 Auto Scaling generates events when it launches or terminates instances. 

* Rules - matches incoming events and routes them to targets for processing

* Targets - processes events. Targets can include Amazon EC2 instances, AWS Lambda functions, Kinesis streams, Amazon ECS tasks, Step Functions state machines, Amazon SNS topics, Amazon SQS queues, and built-in targets. A target receives events in JSON format. A rule's targets must be in the same Region as the rule.

### The load balancer is highly available and its public IP may change. The DNS name is constant
* When your load balancer is created, it receives a public DNS name that clients can use to send requests. The DNS servers resolve the DNS name of your load balancer to the public IP addresses of the load balancer nodes for your load balancer. Never resolve the IP of a load balancer as it can change with time. You should always use the DNS name.


### Secret Strings used in application to be encrypted to prevent exposing as clear text. The solution requires that decryption events be audited and API calls to be simple
* Audit using CloudTrail - is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account.

* Store the secret as SecureString in SSM Parameter Store - With AWS Systems Manager Parameter Store, you can create SecureString parameters, which are parameters that have a plaintext parameter name and an encrypted parameter value. Parameter Store uses AWS KMS to encrypt and decrypt the parameter values of Secure String parameters. Also, if you are using customer-managed CMKs, you can use IAM policies and key policies to manage to encrypt and decrypt permissions. To retrieve the decrypted value you only need to do one API call.


### SSE-S3, SSE-KMS, SSE-C
* SSE-S3 - requires that **Aamazon S3** manage the data and encryptionn keys. If you don't want to manage encryption keys
* SSE-C - requires that **you** manage the encryption keys 
* SSE-KMS - requires that **AWS** manage the data key but **you** manage the Customer Managed Key (CMK) in AWS KMS
* Client Side Encryption -  You can encrypt data client-side and upload the encrypted data to Amazon S3. In this case, you manage the encryption process

### Valid headers for SSE-S3
```
'x-amz-server-side-encryption': 'AES256'
```
- Amazon S3 server-side encryption uses one of the strongest block ciphers available to encrypt your data, 256-bit Advanced Encryption Standard (AES-256).

### Valid header for SSE-KMS
```
'x-amz-server-side-encryption': 'aws:kms'
```
- Server-side encryption is the encryption of data at its destination by the application or service that receives it. AWS Key Management Service (AWS KMS) is a service that combines secure, highly available hardware and software to provide a key management system scaled for the cloud. Amazon S3 uses AWS KMS customer master keys (CMKs) to encrypt your Amazon S3 objects. AWS KMS encrypts only the object data. Any object metadata is not encrypted.

This is a valid header value and you can use if you need more control over your keys like create, rotating, disabling them using AWS KMS. Otherwise, if you wish to let AWS S3 manage your keys just stick with SSE-S3.


### EC2 detailed monitoring vs basic monitoring 
* Basic Monitoring - EC2 sends metric to CloudWatch in **5 minute periods** No charge
* Detailed Monitoring - EC2 sends metric to CloudWatch in **1 minute period**. This requires additional costs

### Pre-Signed URL in S3
* All objects by default are private, with object owner having permission to access the objects. However, the object owner can optionally share objects with others by creating a pre-signed URL, using their own security credentials, to grant **time-limited** permission to download the objects.

### Fanout feature of Kinesis Data Streams 
* You should use enhanced fan-out if you have **multiple consumers retrieving data from a stream in parallel** 

### What is CORS
* Cross-origin resource sharing (CORS) defines a way for client web applications that are loaded in one domain to interact with resources in a different domain. With CORS support, you can build rich client-side web applications with Amazon S3 and selectively allow cross-origin access to your Amazon S3 resources.

To configure your bucket to allow cross-origin requests, you create a CORS configuration, which is an XML document with rules that identify the origins that you will allow to access your bucket, the operations (HTTP methods) that will support for each origin, and other operation-specific information.

For the given use-case, you would create a `<CORSRule>` in `<CORSConfiguration>` for bucket B to allow access from the S3 website origin hosted on bucket A.

### If requires the users to login every after deployment 
* Use ElastiCache to maintain user sessions

### Efficient way to re-create all records from DynamoDB with minimal cost
* Delete then re-create the table 

### Associating, Describing and Creating KMS key using CLI
* Associating - Log group data is always encrypted in CloudWatch logs, to use the KMS and associate CMK to **existing** log group you can use: 
```
=> associate-kms-key
=> aws logs associate-kms-key --log-group-name my-log-group --kms-key-id "key-arn" // full command
```

* Describing - to find whether a log group already has a CMK associated with it. 
```
=> describe-log-groups
=> aws logs describe-log-groups --log-group-name-prefix "log-group-name-prefix"
```

* Creating - command to associate the CMK with a log group when you create it.
```
=> create-log-group
=> aws logs create-log-group --log-group-name my-log-group --kms-key-id "key-arn"
```

### Rules supported in Load Balancer for re-routing 
* Path Based - allows you to route requests to, for example, /api to one set of servers (also known as target groups) and /mobile to another set. Segmenting your traffic in this way gives you the ability to control the processing environment for each category of requests.

* Host Based - Requests to api.example.com can be sent to one target group, requests to mobile.example.com to another, and all others (by way of a default rule) can be sent to a third. 

* Other Supported routing strategies:
https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html#rule-condition-types