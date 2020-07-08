# Fargate

* Serverless way to manage EC2 instances
* We don't provision EC2 instances
* We just create task definitions and AWS will run our container for us
* To scale, just increase the task number
* ECS creates EC2 instances and you manage it by adding more or less EC2 instances


## Three types of cluster templates
* Networking (Fargate) 
  * Cluster
  * VPC 
  * Subnets 

* EC2 Linux + Networking 
  * Cluster
  * VPC
  * Subnets
  * Auto Scaling group with Linux AMI

* EC2 Windows + Networking
  * Cluster
  * VPC
  * Subnets
  * Auto Scaling group with Windows AMI