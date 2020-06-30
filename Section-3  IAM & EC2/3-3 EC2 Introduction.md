### EC2 Introduction

It allows you to launch virtual machines in the cloud

EC2 Consists of the following:
* EC2 - virtual machine in AWS
* EBS - Storing of data on virtual drives
* ELB - Distributing load accros machines
* ASG - Scaling the services using an Auto Scaling Group 

When Launching EC2:
* **AMI** - a template that contains the operating system, application server and applications (Amazon Linux, Ubuntu Server)
* **Instance Types** - How powerful your machine you want to be (t2.micro, t2.nano ...)
* **Configure Instance** - allows you to choose networks, number of instances, create subnet...
* **Storage** - storage device settings by default the volume is root
* **Tags** - key/value pair to describe EC2 instance
* **Security Group** - firwalls for EC2 instance. How do you want your EC2 to be accessed e.g. port 22 ssh, 80 HTTP

Note: EC2 can be accessed from machine by ssh key pair of an EC2 instance