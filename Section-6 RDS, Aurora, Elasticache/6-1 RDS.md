# RDS - Relational Database Service

* It is a managed DB service for SQL
* Supported Databases
  * PostgreSQL
  * Oracle 
  * MySQL
  * MariaDB
  * Microsoft SQL Server
  * Aurora (AWS Owned Database)
* Scales vertical and horizontal
* Read replicas for improved performance
* Multi AZ setup for Disaster Recovery
* RDS instances have security group


## RDS Read Replicas for read scalability
* Up to 5 read replicas
* Within AZ, Cross AZ, or Cross Region
* Replication is ASYNC 
* Replicas can be promoted to their own DB
* Only 1 master takes the **writes**, replicas takes the **reads**

## RDS Multi AZ (Disaster Recovery)
* SYNC Replication
* Not used for sacaling 
* One DNS name - automatic app failover to standby
* increase availability

## RDS Backups 
* Backups are automatically enabled in RDS
* Automated Backups 
  * Daily full snapshot of database
  * Capture transaction logs in real time
  * 7 Days retention (can be increase to 35 days)
* DB Snapshots
  * Manually triggered by the user
  * Retention of backup for as long as you want

## RDS Encryption
* Encryption at rest with AWS KMS - AES 256 encryption
* SSL Certs to encrypt data to RDS in flight
* To enforce SSL:
  * PostgreSQL - *rds.force_ssl=1* in AWS RDS Console
  * MySQL - within the DB: *GRANT USAGE ON *.* TO 'MYSQLUSER@%' REQUIRE SSQL*
* Connecting using SSL is necessarily enforced

### RDS vs Aurora
* Aurora is AWS owned not open source
* Aurora 5x performance improvement over MySQL on RDS and 3x performance over Postgres
* Aurora costs more than RDS (20% more)
* **Aurora Global Database** - allows you to have cross region replication