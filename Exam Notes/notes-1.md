# Exam Notes Part 1

### Steps that can take high CPU load off the web servers
  * Create HTTPS listener on the ALB with SSL termination
  * Configure an SSL/TLS certificate on an ALB via AWS Certicate Manager (ACM)

### To Troubleshoot the issue and to see which commands are causing an error on CodeBuild 
  * You can run AWS CodeBuild Locally 

### Can Store and Deploy Docker Images
  * ECR

### To access Lambda using 3rd party authorization mechanism:
  * Lambda Authorizer

### To deploy new application versions to new EC2 instances while serving request to old application in ELB
  * Rolling Deployment - update few instances at a time, and then move onto next bucket once bucket is healthy
  * Others:
    * All at Once - fastest but instances aren't available to serve traffic for a bit (downtime)
    * Rolling with Additional Batches - Like rolling, but **spins up new instances** to move the batch (so that the old application is still available)
    * Immutable 
      - spins up new instances in new ASG, deploys version to these intstances and then **swaps all instances when everything is healthy**
      - ensures that your new application version is always deployed to new instances, instead of updating existing instances. It also has the additional advantage of a quick and safe rollback in case the deployment fails

### If you want the SQS queue to extend the process
  * Use `ChangeMessageVisibility` action to extend a message's visibility

### What record should you create in Route 53 if you want to point the URL to URL 
  * Use `CNAME` - URL to URL
  * Others:
    * A - URL to IPv4       
    * AAAA - URL to IPv6
    * Alias - URL to AWS Resource (ALB)

### Which of the following credentials is NOT supported by IAM for CodeCommit?
  * IAM username and password

### Which of the following is NOT help with the debugging in EC2 when failing to send data to X-Ray
  * X-Ray Sampling 

### Invalid Section of the CloudFormation template
  * Dependencies 

### To audit activities in following services
  * CloudTrail

### If the CodeBuild taking too long to build because of dependencies
  * Enable CodeBuild timeouts

### Not true about Auto Scaling 
  * An auto scaling group can contain EC2 in only one AZ of a Region
  * ASG groups that span across multiple regions need to be enabled for all the regions specified

### To Map all possible values for the base AMI using `!FindInMap`
  * `!FindInMap [MapName, TopLevelKey, SecondLevelKey]`

### To Debug and trace data across accounts and visualize it in a centralized account
  * AWS X-Ray

### One employee, in us-east-1 Region, has created a stack 'Application1' and made an exported output with the name 'ELBDNSName'. Another employee has created a stack for a different application 'Application2' in us-east-2 Region and also exported an output with the name 'ELBDNSName'. The first employee wanted to deploy the CloudFormation stack 'Application1' in us-east-2, but it got an error. What is the cause of the error?
  * Exported Output values in CloudFormation must have a unique names within a single region

### which of the following IAM constructs would you recommend so that the policy snippet can be made generic for all team members and the manager does not need to create separate IAM policy for each team member
  * IAM policy variables

### You cannot use SMS text message MFA for root user authentication

### Convertible Reserved Instances - can be exchanged during the term for another Convertible Reserved Instance with new attributes including instance family, instance type, platform, scope, or tenancy

### Limits permissions, but cannot grant permissions
  * AWS Organizations Service Control Policy (SCP)
  * Permissions Boundary

  Others:
  * Resource-based policy - Resource-based policies grant permissions to the principal that is specified in the policy. Principals can be in the same account as the resource or in other accounts. The most common examples of resource-based policies are Amazon S3 bucket policies and IAM role trust policies.

  * Identity-based policy - Help attach managed and inline policies to IAM identities (users, groups to which users belong, or roles). Identity-based policies grant permissions to an identity.

  * Access control list (ACL) - Use ACLs to control which principals in other accounts can access the resource to which the ACL is attached. ACLs are similar to resource-based policies, although they are the only policy type that does not use the JSON policy document structure. ACLs are cross-account permissions policies that grant permissions to the specified principal.

### As part of his development work, an AWS Certified Developer Associate is creating policies and attaching them to IAM identities. After creating necessary Identity-based policies, he is now creating Resource-based policies. Which is the only resource-based policy that the IAM service supports?
  * Trust Policy

### As an AWS Certified Developer Associate, you been asked to create an AWS Elastic Beanstalk environment to handle deployment for an application that has high traffic and high availability needs. You need to deploy the new version using Beanstalk while making sure that performance and availability are not affected. Which of the following is the MOST optimal way to do this while keeping the solution cost-effective?
  * Deploy using Rolling with additional batches

### Valid Serverless Application Model (SAM)
  * AWS::Serverless::Api
  * AWS::Serverless::Function
  * AWS::Serverless::SimpleTable

### HTTP Statuses Error:
  * 504 -  HTTP 504 is 'Gateway timeout' error. Several reasons for this error, to quote a few: The load balancer failed to establish a connection to the target before the connection timeout expired, The load balancer established a connection to the target but the target did not respond before the idle timeout period elapsed.
  * 503 - HTTP 503 indicates 'Service unavailable' error. This error in ALB is an indicator of the target groups for the load balancer having no registered targets.
  * 500 - HTTP 500 indicates 'Internal server' error. There are several reasons for their error: A client submitted a request without an HTTP protocol, and the load balancer was unable to generate a redirect URL, there was an error executing the web ACL rules.
  * 403 - HTTP 403 is 'Forbidden' error. You configured an AWS WAF web access control list (web ACL) to monitor requests to your Application Load Balancer and it blocked a request.

### EC2 Launch Types
* On demand instances - short workload, predictable pricing
* Reserved - minimum 1 year, Convert
  * Reserved Instances - long workloads
  * Convertible Reserved Instances - long workloads with flexible instances
  * Scheduled Reserved Instances - e.g. every Thursday between 3-6 pm

* Spot Instances - short workloads, for cheap, can lose instances and less reliable
* Dedicated Instances - no other customers will share your hardware
* Dedicated Hosts - book physical server, control instance placement

### Can be used to deploy/use SSL/TLS certs
  * IAM
  * ACM - AWS Certificate Manager

### CloudFormation template does not allow for conditions  
  * Parameters - enable you to input custom values to your CloudFormation template each time you create or update a stack.
  * Others:
    * Resources - describes the resources that you want to provision
    * Conditions - defines conditions in CloudFormation Template
    * Outputs - declares output values that you can import into other stacks

### Store your secrets securely and automatically rotate the database credentials.
  * Secrets Manager - enables you to easily rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle
  * Others
    * SSM Param Store - provides secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, and license codes as parameter values. SSM Parameter Store cannot be used to automatically rotate the database credentials.
    * Systems Manager -  gives you visibility and control of your infrastructure on AWS. Systems Manager provides a unified user interface so you can view operational data from multiple AWS services and allows you to automate operational tasks across your AWS resources
    * KMS - makes it easy for you to create and manage cryptographic keys and control their use across a wide range of AWS services and in your applications.
  

### identify unused IAM roles and remove them without disrupting any service
* Access Advisor feature on IAM console

### You need to implement an authentication mechanism that returns a JWT (JSON Web Token).
  * Congito User Pools
  * Others:
    * Cognito Sync - Amazon Cognito Sync is an AWS service and client library that enables cross-device syncing of application-related user data. You can use it to synchronize user profile data across mobile devices and the web without requiring your own backend.
    * Cognito Identity Pools - You can use Identity pools to grant your users access to other AWS services. With an identity pool, your users can obtain temporary AWS credentials to access AWS services, such as Amazon S3 and DynamoDB. Identity pools support anonymous guest users, as well as the specific identity providers that you can use to authenticate users for identity pools.

### AWS Kinesis 
  * Kinesis Data Streams - is a massively scalable and durable real-time data streaming service. KDS can continuously capture gigabytes of data per second from hundreds of thousands of sources such as website clickstreams, database event streams, financial transactions, social media feeds, IT logs, and location-tracking events 

  * Kinesis Data Firehose - is the easiest way to **analyze streaming data** in real-time. 

### CI / CD 
  * CodeDeploy - automates application deployments to EC2 instances, supports Blue / Green Deployment 
  * CodeBuild - fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy. It cannot be used to deploy applications.
  * EBS -  performs an in-place update when you update your application versions, your application can become unavailable to users for a short period of time. Y
  * CodePipeline - automates the build, test, and deploy phases of your release process every time there is a code change. CodePipeline by itself cannot deploy applications.

### Logging
  * ALB Access Logs - Elastic Load Balancing provides access logs that capture detailed information about requests sent to your load balancer. 
  * CloudTrail Logs - a service that provides a record of actions taken by a user, role, or an AWS service in Elastic Load Balancing
  * CloudWatch Metrics - enables you to retrieve statistics about those data points as an ordered set of time-series data, known as metrics.
  * ALB Request Tracking - You can use request tracing to track HTTP requests. The load balancer adds a header with a trace identifier to each request it receives

### ELB (Elastic Load Balancer) Characteristics
  * highly available system
  * Separate public traffic from private traffic


### Two docker containers using shared memory
  * Put two containers into a single task definition using Fargate Launch Type

### helps you identify the resources in your organization and accounts, such as Amazon S3 buckets or IAM roles, that are shared with an external entity. This lets you identify unintended access to your resources and data, which is a security risk.
  * IAM Access Analyzer

### How to give other users to access Billing and Costs
  * You need to go to Billing and Cost Management console and activate a user who needs access

### KMS 
  * KMS stores the CMK and receives the data from the clients, which it encrypts and sends it back

### No alerts have been received event though the account and the budget has been created 
  * AWS requieres approx 5 weeks of usage to generate budget forecasts

### Developer wants to focus on writing application code without worrying about the server provision on EC2, configuration and deployment 
* Elastic Beanstalk - provides an environment to easily deploy and run applications in the cloud.
* Others:
    * CloudFromation - is a service that gives developers and businesses an easy way to create a collection of related AWS and third-party resources and provision them in an orderly and predictable fashion. 
    * Serverless Application Model (SAM) - is an open-source framework for building serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings. You define the application you want with just a few lines per resource and model it using YAML. As the web application needs to be deployed on EC2 instances, so this option is ruled out.

### Which of the following security credentials can only be created by the AWS Account root user?
  * CloudFront Key Pairs - IAM users can't create CloudFront key pairs. You must log in using root credentials to create key pairs.

### A developer at a company is trying to create a digital signature for SSH'ing into the Amazon EC2 instances. Which of the following entities can be used to facilitate this use-case?
  * Key Pairs 


### Computing WCU and RCU in DynamoDB
  * Check https://github.com/ChunAllen/AWS-CDA/blob/5fd4de2d7c6ab2aa1f343ffca80bf15ef5fd9828/Section-20%20AWS%20DynamoDB/20-1%20DynamoDB.md#write-capacity-units