# Advance S3 and Athena

## S3 Replication (CRR & SRR)

* CRR - cross region replication
  * Use cases: compliance, lower latency access, replication across accounts

* SRR - same region replication
  * Use cases: Log aggregation, live replication
  between production and test accounts

Note: 
  * When replicating from region 1 bucket to another region it is using asynchronous replication
  * Must give proper IAM permission to s3
  * Buckets can be different accounts
  * After activating, only new objects are replicated
  * If you delete without a version ID - it adds delete market, not replicated
  * If you delete with a version ID - it deletes in the source, not replicated
  * There is no chaning of replication
  * it is recommended to enable versioning in both buckets (main and target)
  * add a replication rule
  * Add IAM role to enable ReplicateObject, ReplicateDelete, ReplicateTags

## S3 pre-signed URLs
* Can generate URLS using SDK or CLI
* valid for a default 3600 seconds, can change timeout with --expires-in[TIME_BY_SECONDS] argument
* Users given a pre-signed URL inherit the permission of the person who generated the URL for GET/PUT
* Use cases:
  * premium video
  * allow temporily a user to upload a file to a precise location in our bucket


## Storage Classes
* **S3 Standard-General Purpose**
  * High durability (99.99999%) of objects across multiple AZ
  * If you store 10,000.000 objects in s3, you can on avarage to incur a loss of a single objects once every 10,000 years
  * Use cases: Big Data analytics, mobile and gaming applications, content distribution

* **S3 Standard-Infrequent Access (IA)**
  * suitable for data that is less frequently accessed, but requires rapid access when needed
  * Use cases: as a data store for disaster recovery, backups...

* **S3 One Zone-Infrequent Access**
  * same as AZ but data is stored in single AZ
  * Storing secondary backup copies of on-premise data or storing data you can recreate

* **S3 Intelligent Tiering**
  * Same low latency and high throughput performance of s3 standard
  * Designed for 99.9% availablity over a given year

* **S3 Glacier**
  * Low cose object storage meant for archiving / backup
  * Data is retained for longer term (10s of years)
  * Alternative to on-premise magnetic tape storage
  * Archives are stored in Vaulds
  * If you want to store data for long term and you will not access frequently
  * minimum storage duration of 90 days

* **S3 Glacier Deep Archive**
  * minimum storage duration of 180 days
  * longer that s3 Glacier


## Amazon Athena
  * allows you to perform SQL queries in of s3 files
  * supports to perform analytics directly against s3 files
  * Analyze data **directly** on s3
  
## S3 Select and Glacier Select
  * Retreive less data using SQL by performing server side filtering 
  * Can filter by rows & columns (simple SQL statements)

## S3 Event Notifications
  * Available targets to be notified when there are changes with the s3 bucket
    * SNS - notifications or email
    * SQS - Queue Service
    * Lambda - Custom Code

  * S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication

  * You can create as many s3 events as desired
