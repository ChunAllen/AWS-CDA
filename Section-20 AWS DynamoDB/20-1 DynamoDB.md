# AWS DynamoDB 

* Serverless NoSQL Database by AWS
* NoSQL databases scale horizontally
* DynamoDB is made of **tables**
* Each table has **primary key** which is called **partition key**(must be decided at creation time)
* Each table can have infinite number of items (= rows)
* Each item has **attributes**  
* Maximum size of an item is **400KB**
* Data Types supported are:
  * Scalar Types - String, Number, Binary, Boolean, Null
  * Document Types: List, Map
  * Set Types - String Set, Number Set, Binary Set
* Partition Key can only be string, binary, number
* Sort Key == range key
* Any attributes can be a null except partition key
* If your primary key is combination of `partition key` and `sort key` them both of them **should** be unique
* Which concurrency model can be implemented with DynamoDB? - Optimistic Locking
* Which feature of DynamoDB allows it to achieve Optimistic Locking? - Conditional Writes

## To Reduce Latency
* Use **eventually consistent reads** over **strongly consistent reads**
* Consider using Global Tables if you application is accessed by global distributed users

## Throughput 
* Read Capacity Units (RCU) - throughput for reads
* Write Capacity Units (WCU) - throughput for writes
* Option to setup auto-scaling of througput to meet demand
* Throughput can be exceeded temporarily using **burst credit**
* If burst credit are empty, you'll get a `ProvisionedThroughputException`
* We are getting a ProvisionedThroughputExceededExceptions but after checking the metrics, we see we haven't exceeded the total RCU we had provisioned. What happened?
  * it's because we have a hot partition key / hot key

## Write Capacity Units
* Is calculated per second * 1kb in size
* If objects per is not in seconds, you need to convert it to seconds (1min = 60 seconds)

**For Example:**
* 10 objects per seconds of 2kb
  ```=> 10 * 2 = 20 WCU```
* 6 Objects per seconds of 4.5KB 
  ```6 * 5 = 30 WCU```
  Note: KB will be rounded up

* 120 Objects per minute of 2KB 
  ``` 120 / 60 * 2 = 4 WCU```

## Read Capacity Units
* ```1 Consistent Read``` is ``2 Eventually Consistent Read``
* 4KB per size for 1 strongly consistent
* Always divite the eventually consistent to strong consistent (n/2)
* If KB is not in 4KB round it up by 2 e.g. 6KB will be 8KB. It should be divisible by 4KB

**Formula**
* strong consistent read * (KB / 4kb))
* (eventually consistent / 2) * (KB / 4KB)
* always convert eventually to strong (eventually / 2)

**For Example**
* Item Size: 6 KB
* Item Read: 4 Eventually Consistent
* Item Write: 7

```
=> 7 * 6KB = 42 WCU
=> (4 / 2) * (8KB / 4) = 4RCU
```
-------------------------------

* Item Size: 6KB
* Item Read: 4 Strong Consistent
* Item Write: 7 

```
=> 7 * 6KB = 42 WCU
=> 4 * (8 / 4KB) = 8 RCU
```
-------------------------------
* Item Size: 10KB
* Item Read: 10 Strong Consistent
* Item Write: 120

```
=> 10 * (12 / 4KB) = 30 RCU (because 10kb will be rounded up to 12KB)
=> (120 / 60) * 10KB = 20 WCU
```
---------------------------------
* Item Size: 16KB
* Item Read: 12 Eventually Consistent
* Item Write: 6

```
=> 6 * 16KB = 96 WCU
=> (12 / 2) * (16KB / 4KB ) = 24 RCU
```


## Strongly Consistend Read vs Eventually Consistent Read
* Eventually COnsistent Read - If we read just after a write, it's possible we'll get unexpected response because of replication
* Strongly Consistent Read - If we read just after a write, we will get the correct data
* By Default - DynamoDB uses Eventually Consisten Reads, but `GetItem` query and `Scan` provide a `ConsistentRead` parameter you can set to True


## DynamoDB Basic APIs
* ProjectionExpression
  * select the attributes to retrieve in the response while using the GetItem DynamoDB CLI. This is equivalent to `SELECT name, address FROM users` rather than `SELECT * FROM users`

* PutItem 
  * write data to DynamoDB. create data of full replace
  * Consumes WCU

* UpdateItem - Update data in DynamoDB (partial update of attributes)
  * Possibility to use Atomic Counters and increase them

* Conditional Writes
  * Accept a write / update only if conditions are accepted 
  * Helps with concurrent access to items
  * No performance Impact

* DeleteItem
  * Delete an individual row
  * ability to perform a conditional delete

* DeleteTable 
  * Delete a whole table and all its items
  * much quicker deletion that calling DeleteItem on all items

* BatchWriteItem 
  * Up to 25 PutItem and / or DeleteItem in one Call
  * Up to 16MB of data written
  * Up to 400Kb of data per item
  * batching allows you to save in latency by reducing the number of API calls done against DynamoDB

* Query
  * returns item based in:
    * PartitionKey value (must be = operator)
    * SortKey value = (=, <, <=, >, >=, Between, Begin) - optional
    * FilterExpression to further filter 
  * Returns 
    * up to 1MB of data
    * Or number of item specified in Limit
  * Able to do pagination on results
  * Can query table, a local secondary index, or a global secondary index

* Scan 
  * Scan the entire table and filter of data (inefficient)
  * Returns up to 1MB of data - use pagination to keep in reading
  * Consumes a lot of RCU
  * For faster perfomance, use **parrallel scans**
    * Multiple instances scan multiple partitions at the same time
    * Increases the throughput and RCU consumed
    * Limit the impact of parallel scans 
  * Can use a `ProjectionExpression` + `FilterExpression` (no change to RCU)

## DynamoDB Local Secondary Index
* Alternate range key for your table, local to hash key
* Up to five local secondary indexes per table
* The sorty key consists of exactly on scalar attribute
* The attribute that you choose must be String, Number, or Binary
* LSI must be defined at table creation time
* Should be an attribute in a table
* Allows you to query LSI 
* You would like to have query for a non key attribute for the >= predicate while keeping the same partition key. You should, Use LSI

## Global Secondary Index
* To speed up queries on non-key attributes, use GSI
* GSI = partition key + optional sort key
* The index is new table and we can project attributes on it
* Must defined RCU / WCU for the index
* Possibility to add / moditfy GSI (not LSI)
* You want to use Query equal operation on a non key attribute. How can you do it? use GSI
* GSI have an independent amount of RCU and WCU and if they are throttled due to insufficient capacity, then the main table will also be throttled


## DynamoDB DAX (DynamoDB Accelerator)
* Enables to cache reads and writes
* Seamless cache for DynamoDB, no application re-write
* Writes go through DAX to DynamoDB
* Multi AZ (3 nodes minimum recommended for production)
* Secure - Encryption at rest with KMS, VPC, IAM, CloudTrail

## DynamoDB Streams
* Changes in DynamoDB (Create, Update, Delete) can end up in DynamoDB Stream
* Can react to changes real time 
* Analytics
* Create derivative tables / views
* Inser into ElasticSearch
* Could implement cross region replication using stream
* Steam has 24 hours of data retention
* Streams can be integrated with AWS Lambda if there are any changes from your items in dynamo db table
* Once you created streams you need to create trigger tie up to the stream

## DynamoDB - TTL (Time to Live)
* automatically delete an item after an expiry date / time
* is provided at no extra cost, deletions do not use WCU / RCU
* is background task operated by DynamoDB itself
* helps reduce storage and manage the table size over time
* the item will be deleted based on **expire_on** attribute or given attribute

## DynamoDB CLI - Good to know
* `--projection-expression` - attributes to retrieve, allow you to retrieve a subset of the attributes coming from a DynamoDB scan
* `--filter-expression` - filter results
* General CLI pagination options including DynamoDB / S3
  * Optimization:
    * `--page-size` - fill dataaset is still received but each API call will request less data
  * Pagination
    * `--max-items` - max number if results returned by CLI
    * `--starting-token` - specify the last received NextToken to keep in reading
* You would like to paginate the results of a DynamoDB scan in order to minimize the amount of RCU that you will use for that CLI command. Which CLI options should you use? -- use `--max-items & --starting-token`

## DynamoDB Transactions
* Ability to create / update / delete multiple rows in different tables at the same time
* Consume 2x of WCU and RCU
