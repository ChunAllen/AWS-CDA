# Lambda Part 3

## Lambda Destinations
* Asynchronous Invocations - can define destinations for successful and failed events
  * SQS
  * SNS
  * Lambda
  * EventBridge bus
* Note: AWS recommends you use destinations instead of DLQ now
* Event Source Mapping - for discarded event batches
  * SQS
  * SNS

## Lambda Execution Role
* Grants the Lambda function permissions to AWS services / resources
* Sample managed policies for Lambda
  * AWSLambdaBasicExecutionRole - Upload logs to CloudWatch 
  * AWSLambdaKinesisExecutionRole - Read from Kinesis
  * AWSLambdaDynamoDBExecutionRole - Read from DynamoDB streams
  * AWSLambdaSQSQueueExecutionRole - Read from SQS
  * AWSLambdaVPCAccessExecutionRole - Deploy Lambda function in VPC
  * AWSXRayDaemonWriteAccess - Upload trace data to X-Ray

* When you use an **event source mapping** to invoke your function, Lambda uses the execition role to read event data
* Best Practive: create one Lambda Execution Role per function

## Lambda & CodeDeploy
* CodeDeploy can automate traffic shift for Lambda aliases
* Types of deployment:
  * Linear - grow traffic every N minutes untile 100%
  * Canary - try X percent then 100%
  * AllAtOnce - immediate