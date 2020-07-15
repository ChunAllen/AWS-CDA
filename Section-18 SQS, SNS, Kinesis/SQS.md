# SQS - Simple Queue Service

* Processes background jobs for our applications. For e.g. Producers sends messages to SQS queue then Consumers polling messages from SQS Queue
* Scales from 1 message per second to 10,000s per second
* Default retention of messages **4 days, maximum of 14 days**
* Horizontal scaling in terms of number of consumers
* Can have duplicate messages 
* Can have out of order messages
* Limitation of 256kb per message sent

## SQS - Delay Queue
* Delay a message (consumers don't see it immediately) up to 15 minutes
* Defailt is 0 seconds (message is available right away)
* Can set a default at queue level
* Can override the default using the `DelaySeconds` parameter

## SQS - Producing Messages
* Define Body
* Add message attributes (metadata - optional)
  * For e.g. Name, Type, Value
* Provide Delay Delivery (optional)

* Get Back:
  * Message identifier
  * MD5 Hash of the body

## SQS - Consuming Messages
* Messages where consumed by **consumers**
* Poll SQS for messages (receive up to 10 messages at a time)
* Process the message within the visibility timeout
* Delete the message using the message ID & Receipt handle

## SQS - Visibility Timeout
* Visibility Timeout - when consume polls a message from a queue, the message is "invisible" to other consumers for a defined period
  * Set between 0 seconds and 12 hours (default 30 seconds)

* ChangeMessageVisibility - API to change the visibility while processing a message
* DeleteMessage - API to tell SQS the message was successfully processed


## SQS - Dead Letter Queue (DLQ)
* Contains all the messages that are all problematic
* We can set a threshold of how many times a message can go back to the queue it's called **redrive policy**
* After the threshold is exceeded, the message goes into a dead letter queue (DLQ)
* We have to create a DLQ first and then designate it dead letter queue
* Make sure to process the messages in the DLQ before they expire

## SQS - Long Polling
* When a consumer requests message from the queue, it can optionally **wait** for messages to arrive if there are none in the queue - this is called **Long Polling**
* LongPolling decreases the number of API calls made to SQS while increasing the efficiency and latency of your application
* THe wait can be between 1 sec to 20 sec (20 sec preferable)
* Long Polling is preferable to Short Pollng
* Long Polling can be enabled at the queue level or at the API level using **WaitTimeSeconds**


## Need to know
* Purge Queue - deletes all messages from the queue
* Default visibility timeout - number of minutes or seconds will the consumer will process the message before it gets deleted
* Message retention period - default 4 days max 14 days of messages in SQS
* Message can have body (required) or Message Attributes (optional)

## Command for sending SQS Message
```
$ aws sqs send-message --queue-url http://queue.amazonaws.com/123123123/MyFirstQueue --region us-east-1 --message-body hello-world

{
  "MD5OfMessageBody": "123123123123123",
  "MessageId": "asdasd-012asdasdasd-123asdasd"
}
```

## Command for receiving SQS message
```
$ aws sqs receive-message --region us-east-1 --queue-url https://queue.amazonaws.com/123123123/MyFirstQueue --max-number-of-messages 10 --visibility-timeout 30 --wait-time-seconds 20

{
  "Messages": [
    {
      "MessageId": "asdasd-012asdasdasd-123asdasd",
      "ReceiptHandle": "asdhaskdhjkahjsdkhasjkdhk12h3jkh12k3hjhasd",
      "MD5OfBody": "asd123123",
      "Body": "hello-world"
    }
  ]
}
```

## Deleting message for SQS 
```
$ aws sqs delete-message --reciept-handleasdhaskdhjkahjsdkhasjkdhk12h3jkh12k3hjhasd
```

## SQS - FIFO Queue
* First In First Out
* Name of the queue must end in **.fifo**
* Lower throughput (up to 3,000 per second with batching, 300/s without)
* Message are processed in order by the consume
* Messages are sent exactly once
* No permessage delay (only per queue delay)


## SQS - FIFO Features
* Deduplication: (not send the same message twice)
  * Provide a **MessageDeduplicationId** with your message
  * De-duplication interval is 5 minutes

* Sequencing 
  * To ensure strict ordering between messages, specify a **MessageGroupId**
  * E.g. To order messages for a user, you could use the `user_id` as a group id
  * Messages with the same Group ID are delivered to one consumer at a time


## SQS Extented Client
* Producer sends larger message to s3 and smaller meta data message to SQS
* SQS then sends small metadata to Consumer
* Consumer now will retrieve large message from S3

## SQS Security
* Encryption in flight using the HTTPS endpoint
* Can enable SSE (Server Side Encryption) using KMS
* IAM Policy must allow usage of SQS
* SQS queue access policy
* No VPC Endpoint, must have internet access to access SQS

## SQS - Must Know API
* CreateQueue, DeleteQueue
* PurgeQueue - Delete all messages in queue
* SendMessage, ReceiveMessage, DeleteMessage
* ChangeMessageVisibility - change the timeout

* Batch APIs for SendMessage, DeleteMessage, ChangeMessageVisibility helps decrease your costs