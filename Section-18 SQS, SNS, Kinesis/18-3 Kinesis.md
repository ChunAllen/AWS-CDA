# AWS Kinesis

* For Real Time Big Data
* Alternative to Apache Kafka
* Create for application logs, metrics, IOT, clickstreams
* Data is automatically replicated to 3 AZ

## 3 Types of Kinesis
* Kinesis Streams - low latency streaming ingest at scale
* Kinesis Analytics - perform real-time analytics on streams using SQL
* Kinesis Firehose - load streams into s3, RedShift, ElasticSearch

## Kinesis Overview
* Streams are dividewd in ordered Shards / Partitions
* Data retention is 1 day by default, can go up to 7 days
* Ability to reprocess / replay data
* Multiple applications can consume the same stream
* Real time processing with scale of throughput
* Once data is inserted in Kinesis, it can't be deleted (immutability)

## Kinesis Streams Shards
* One stream is made of many different shards
* 1MB/s or 1000 messages at write per shard
* 2MB/s at read per shard
* Billing is per shard provisioned, can have many shards as you want
* Batching available or per message calls
* The number of shards can evolve over time (reshard/merge)
* Records are ordered per shard


## Kinesis API - Put Records
* PutRecord API + Partition key that gets hashed
* The same key goes to the same partition
(helps with ordering for a specific key)
* Messages send get a **sequence number**
* Choose a partition key that is highly distributed (helps prevent hot partition)
  * user_id if many users
  * Not **country_id** if 90% of the users are in one country
* Use batching with PutRecords to reduce costs and increase throughput
* **ProvisionedThroughputExceeded** if we go over the limits

## Kinesis API - Exceptions
* **ProvisonedThroughputExceeded** Exceptions
  * Happens when sending more data 
  * make sure you don't have a hot shard

* Solution:
  * Retries with backoff
  * Increase shards (scaling)
  * Ensure your partition key is a good one


## Kinesis API - Consumers
* Can use a normal consumer (CLI, SDK, etc..)
* Can use Kinesis Client Librariy (In Java, Node Python, Ruby, .NET)
  * KCL uses DynamoDB to chechkpoint offsets
  * KCL uses DynamoDB to track other workers and share the work amongst shards

## AWS CLI - Kinesis
* List Streams
```
$ aws kinesis list-streams
{
  "StreamNames": [
    "my-first-stream"
  ]
}
```

* Describe Kinesis Stream 
```
$ aws kinesis describe-ssteam --stream-name my-first-stream

{
  "StreamDescription": {
    "Shards": [
      "ShardId": "shardId-00000000",
      "HashKeyRange": {
        "StartingHashKey": "0"
      }
    ]
    ...
  }
}
```

* Put Record 
```
$ aws kinesis put-record --stream-name my-first-stream --data "user signup" --partition-key user_123

{
  "ShardId": "shardId-00000",
  "SequenceNumber": "123asdasd123123123"
}
```

* Get Shard data from Kinesis
```
$ aws kinesis get-shard-iterator --stream-name my-first-stream --shard-id shardId-000000 --shard-iterator-type TRIM_HORIZONTRAL

{
  "ShardIterator": "AAAAAasdasdasd"
}
```

* Then use `ShardIterator` to Get Records
```
$ aws kinesis get-records --shard-iterator "AAAAAasdasdasd" 

{
  "Records": [
    {
      "SequenceNumber": "123123123123",
      "ApproximateArrivalTimestamp: "123123123.123123",
      "ParitionKey": "user_123"
    }
  ]
}
``` 

## Kinesis KCL 
* Kinesis Client Library is a Java Library that helps read record from Kinesis Stream with distributed applications sharing the read workload
* Rule: each shard is be read by only one KCL instance
* Means 4 shards = max 4 KCL instances
* 6 shards = max 6 KCL instances
* Progress is checkpointed into DynamoDB
* KCL can run on EC2, Beanstalk, on Premise Application
* Records are read in order at the shard level


## Kinesis Security
* Control access/ autorization using IAM polices
* Encryption in flight using HTTPS endpoints
* Encryption at rest using KMS
* Possibility to encrypt / decrypt data client side (harder)
* VPC endpoints available for Kinesis to access withing VPC

## Kinesis Data Analytics
* Perform real-time analytics on Kinesis Streams using SQL

## Kinesis Firehose
* Fully managed service, no administration
* Near Real Time (60 seconds latency)
* Load data into Redshift / Amazon S3 / ElasticSearch
* Automatic Scaling
* Support manay data format (pay for conversion)
* Pay for the amount of data going through Firehose