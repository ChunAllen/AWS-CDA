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
  
## Understanding ECS 
  
  ### Clusters
   Clusters are a logical way to group resources (services and tasks).
 
  ### Services
   Services are used to run a load balance in front of a group of tasks.
   This is also where you will specify how many instances of a task should be running. The service scheduler is in charge of starting new instances in the case of    an instance failing.

  ### Tasks
  Tasks are the running instances of a task definition.

  ### Task Definitions
  Task Definitions are where you specify the resources for a Docker container or group of containers.
  It is also where you specify the Docker image, any volumes, environment variables, and more.
