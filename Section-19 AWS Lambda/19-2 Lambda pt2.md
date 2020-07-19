# Lambda Part 2

## Synchronous Invocations
* CLI, SDK, API Gateway, Application Load Balancer
* Results is returned right away
* Error handling must happen client side (retries, exponential backoff, etc...)
* Synchrnous Services
  * User Invoked
    * ELB 
    * ALB
    * API Gateway
    * CloudFront (Lambda@Edge)
    * S3 Batch

  * Service Invoked
    * Cognito
    * AWS Step Functions
  * Other Services
    * Amazon Lex
    * Amazon Alexa
    * Amazon Kinesis Data Firehose

```
$ aws lambda invoke --function-name hello-world --cli-binary-format raw-in-base64-out --payload '{"key1": "value1", "key": "value2"}' --regioins eu-west-2 resonse.json
```


## Lambda Integration with ALB
* To expose a Lambda function as an HTTP(s) endpoint
* You can use the ALB or an API Gateway
* The Lambda function must be registered in a **target group**


## ALB Multi-Header Values
* ALB can support multi header values (ALB Setting)
* When you enable multi-value headers, HTTP headers and query string parameters that are sent with multiple values are shown as arrays within the Lambda event and response objects
* Enable multi value headers in target group of Load Balancer

**For Example**
HTTP Request
```
http://example.com.path?name=foo&name=bar
```
The JSON recevie by Lambda will be
```
"queryStringParameters: {"name": ["foo", "bar"]}"
```

Note: It's advisable to have a 1AM role per Lambda function to easier to maintain

## ALB to Invoke Lambda Function
* You need choose a target type as **Lambda Function** when configuring the ALB 
* To handle response from Lambda, the Lambda code will be
```
import json

def lambda_handler(event, context)
  print(event)
  return {
    "statusCode": 200,
    "headers": {
      "Content-Type": "text/html"
    },
    "body": "<h1> Hello from Lambda </h1>"
  }
```
* The Response should be lambda

## Asynchrnous Invocations
* S3, SNS, CloudWatch Events
* The events are placed in an **Event Queue**
* Lambda attempts to retry on errors
  * 3 tries total
  * 1 minute after 1st, then 2 minutes wait

* Make sure procewssing is idempotent (in case of retries)
* If the function is retrieved you will see duplicated entries in CloudWatch Logs
* Can define DLQ - SNS or SQS for failed processing 
* Asynchronous invocations allow you to speed up the processing if you don't need to wait for result

```
$ aws lambda invoke --function-name hello-world --cli-binary-format raw-in-base64-out --payload '{"key": "value1", "key2": "value2" }' --invocation-type Event --regioin eu-west-2 response.json
```

## CloudWatch Events / EventBrige
* EventBridge is CRON of AWS
* It's possible to link EB with Lambda by specifying target in EB
* It's possible to trigget a lambda function every n minute/hour

## Lambda & S3 Event Notfications
* S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3::Replication
* Object name filtering for e.g. `(*jpg)`
* Use case: generate thumbnails of images uploaded to S3
* S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer
* If two writes are made to a single non-versoned object at the same time, it's possible that only be a single event notification will be sent
* If you want to ensure that an event notification is sent for every successful write, you can enable versioning on your bucket
* Create an event in S3 to trigger Lambda Function when object has been added to bucket

## Lambda Event Source Mapping
* Kinesis Data Streams
* SQS & SQS FIFO queue
* DynamoDB Streams

* Common Denominator:
  * records need to be polled from the source
* You Lambda function is invoked synchronously

## Stream & Lambda - Error Handling
* By default, if your function returns an error, the entire batch is reprocssed until the function succeeds, or the items in the batch expire.
* To ensure in-order processing, processing for the affected shard is paused until the error is resolved
* You can configure the event source mapping to:
  * Discard old events
  * Restrict the number of retries
  * Split the batch on error 
* Discareded events can go to a **Destination**

## Lambda - Event Source Mapping SQS & SQS FIFO
* Event Source Mapping will poll SQS (Long Polling)
* Specify batch size (1-10 messages)
* Recommended: Set the queue visibilty timeout to 6x the timeout of your Lambda Function