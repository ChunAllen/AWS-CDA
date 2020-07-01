# EBS Volume

* EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run
* It allows your instances to pesists data
* It is a network drive (not physical drive)
* It's locked to Availability Zone
  * An EBS Volume in us-east-1a cannot be attached to us-east-1b
  * To move a volume across, you need to snapshot it
* Have a provisioned capacity (size in GBs and IOPS)

## EBS Volume Types
* **GP2 (SSD-Cheap)** - general purpose SSD volume that **balances price and performance** for a wide variety of workloads

* **IOI (SSD-Expensive)** - Highest-performance SSD volume for **mission-critical** low latency or high throughput workloads

* **STI (HDD)** - Low cose HDD volume designed for **frequenly accessed**, throughput intensive workloads

* **SCI (HDD)** - Lowest cost HDD volume designed for **less frequently** accessed workloads

## EBS Brain Dump
* EBS can only attached to only one instance at a time
* EBS are locked at the AZ level
* Migrating EBS volume across AZ means first backing it up (snapshot) then recreating it in the other AZ
* EBS backups use IO and you should not run them while your application is handling a lot of traffic
* Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated