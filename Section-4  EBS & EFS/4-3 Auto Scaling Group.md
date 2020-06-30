# Auto Scaling Group

* Scales out (add EC2 instances) to match an increased load
* Scale in (remove EC2 instances) to match decreased load
* Automatically register new instances to a load balancer

## Why use ASG?
* Automated Provisioning
  * Keep your auto scaling group healthy and balanced whether you need one instance of 1,000.
* Adjustable Capacity
  * Maintain a fixed group size or adjust dynamically based on Amazon CloudWatch metrics.
* Launch Template Support
  * Provision instances easily using EC2 Launch Templates.

  ## ASG Attributes
  * Launch Configurations
    * AMI + Instance Type
    * EC2 User Data
    * EBS Volumes
    * Security Groups
    * SSH Key Pair
  
  * Min / Max size and initial capacity
  * Network + Subnets Information
  * Load Balancer Information
  * Scaling Policies

## Auto Scaling Alarms
  * CloudWatch alarms will trigger the scaling depending on policy

## Scaling Policies
  * Dynamic Scaling
    * Target Tracking - increase or decrease the current capacity of the group based on a **target value for specific metric**
    * Step Scaling Policy - increases or decreases the current capacity based on a **set of scaling adjustments** 
    * Simple Scaling Policy - increases or decreases based on **single scaing adjustment**
    
  * Manual Scaling
    * Scheduled Actions - e.g increase min capacity to 10 at 5pm on Friday 