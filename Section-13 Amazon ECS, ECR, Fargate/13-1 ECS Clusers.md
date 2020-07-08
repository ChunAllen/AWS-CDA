# ECS Clusters Overview

* logical **grouping of EC2 instances**
* EC2 instances run the ECS agent (Docker Container)
* ECS agents registers the instance to the ECS cluster

Note:
- ECS Cluser basically creates:
  * EC2 instance Linux OS with amazon-ecs-agent docker container

## ECS Task Definitions
* Task definitions are **metadata** in JSOn form to tell ECS to run a Docker Container
* It contains these informations:
  * Image Name
  * Port Binding for Container and Host
  * memory and CPU required
  * Environment Variables
  * Networking information

* It's like docker-compose.yml
* Task Role is IAM role that is part of Task Definition

## ECS Service
* ECS Services help defined how many tasks should run and how they should be run
* They ensure that the number of tasks desiger is running across or fleet of EC2 instances

Note:
  * Service is tied up to Task Definition, meaning you can have multiple service with 1 task definition
  * Service Type can be:
    * Replica - need to specifiy a number of tasks
    * Daemon - number of task is automatic

## ECS with Load Balancers
If you have load balances in ECS Service your host port will be random to the container port, since you can only have 1 unique host port per container port. 
Load Balancer will help to port forward to the random host port. 

Leaving the host port empty and pointing to the container port will leave the host port random. 

Load Balancer is tied up to the service upon creation.