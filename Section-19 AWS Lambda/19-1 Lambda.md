# AWS Lambda
* Virtual Functions - no servers to manage
* Limited by time - short executions
* Run on-demand
* Scaling is automated
* Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time

* Easy monitory with **CloudWatch** 
* Easy to get more resources (up to 3GB of RAM)

## Need to know
* Use Target Group to integrate with ALB
* Lambda don't use Event Source Mapping to services that are asynchronous
* Use a **direction** to send results of an asynchronous Lambda function to SQS
* Create a **Lambda layer** to compile dependencies so that it will not be slow
* `InvalidParameterValueException` causes of uncompressed zip exceeded the Lambda Limits
* Max 4Kb can be stored in environment variables
* Aliases supports weight on deployment
* Move the connection handler outside of your function handler so that it will not re-establish everytime
* Put the function and dependencies together in one folder and zip them together - to bundler your lambda function and dependencies
* Use local directory `/tmp` to store temporary files and will be discareded when function stops
* CloudWatch Events to use Cron Job with Lambda

## IAM permission to allow Lambda to show logs in CloudWatch
```
{
  "Statement": [
    { 
      "Effect": "Allow",
      "Action": "logs:CreatelogGroup",
      "Resources": "arn:aws:logs:us-east-2:123123123"
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreatelogStream",
        "logs:PutLogEvents"
      ],
      "Resources": "arn:aws:logs:us-east-2:123123123"
    }
  ]
}
```


## Lambda Configuration
* Timeout: default is **3 Seconds**, max of **15 minutes (900s)**
* Supports Environment variables
* Allocated memory (128M to 3G)
* Ability to deploy within a VPC + assign security group
* IAM Execution role must be attached to the lambda function

## Handler Format
* Handler - e.g. lambda_function.lambda_handler is the name of the function
**file: lambda_function**
```
def lambda_handler(event, context)
  print("Hello World")
```

## Defining Environment Variables 
* Environment variables can be defined in Environment Variables section of AWS Lambda function
* to call an environment variable in lambda code
```
os.gotenv('environment_name')
```
where `environment_name` is environment variable 

## Throttling and Concurrency
* Concurrency - up to 1000 executions (can increase through ticket)
  * Can set a **reserved concurrency** at the function level
  * Each invocation over the concurrency limit will trigger a **Throttle**

* Throttle behavious:
  * If synchronous invocation - returns ThrottleError - 429
  * If asynchronous invocation - retry automatically and then go to DLQ

## Lambda Retries and DLQ
* If lambda function asynchronous invocation fails, it will be retried twice
* After all retries, unprocessed events go to the Dead Letter Queue
* DLQ can be a SNS topic of SQS queue

## Logging and Monitoring
* CloudWatch
  * Lambda execution logs are stored in AWS CloudWatch Logs
  * Lambda metrics displayed in AWS CloudWatch Metrics

* X-Ray
  * Enable in Lambda confuration (runs the X-Ray daemon for you)
  * Use AWS SDK in code

  ## Lambda Limits to know
  * Execution 
    * memory allocation: 128 MB to 3008 MB (64 MB increments)
    * Maximum execution time: 300 seconds (5 minutes old - 15 minutes new)
    * Disk capacity in thhe function container (in/tmp): 512 MB
    * Concurrency limits: 1000

  * Deployment 
    * Lambda function deployment size: 50 MB compressed zip
    * Size of uncompressed deployment: Code + Dependencies 250 MB
    * Can use the **/tmp** directory to load other files at startup
    * Size of environment variables - 4KB


## Lambda Versions
* `$LATEST` - default published Lambda Version
* Versions are immutable
* Versions have increasing numbers
* Versions get their own ARN (Amazon Resource Name)
* Version includes code and configuration 
* Each version of the lambda function can be accessed

## Lambda Aliases
* Aliases are pointers to lambda function versions
* dev, test, prod aliases have them point at different versions
* Aliases are mutable
* Aliases enable Blue / Green deployment by assigning weights to lambda functions
* Aliases have their own ARN
* When defining new alias you need to provide
  * name
  * description
  * version

## Lambda Function Dependencies
* Lambda depends on external libraries for eg. AWS X-Ray, SDK, Database Clients, etc...
* You need to install the packages alongside your code and zip it together
  * use npm & "node_modules" directory

## Lambda CloudFormation
* You must store the lambda in S3
* You must refer the S3 zip location in the CloudFormation code

```
AMIIDLookup:
  Type: "AWS::Lambda::Function"
  Properties: 
    Handler: "index.handler"
    Role: :
      Fn::GetAtt:
        - "LambdaExecutionRole"
        - "Arn
    Code: 
      S3Bucket: "lambda-functions"
      S3Key: "amilookup.zip"
```

* Upload the CloudFormation template in CloudFormation that points to s3 as a source of Lambda function

## Lambda /tmp space
* Max size is 512 MB
* The directory content remains when the execution context is frozen
* For permanent persistence of objects use S3

## Best Practices
* Perform heavy duty work outside of your function handler
* Use environment variables for 
  * Database Connection Strings
  * S3 Bucket
  * Passwords, sensitive values

* Minimize your deployment  package size to its runtime necessities
* Avoid using recursive code, never have a Lambda function call itself


## Lambda@Edge
* global AWS lambda
* deploy Lambda alongside CloudFront CDN
