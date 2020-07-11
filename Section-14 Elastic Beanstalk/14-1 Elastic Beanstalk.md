# ElasticBeansStalk Overview

* is a developer centric view of deploying an application on AWS
* It uses these components:
  * EC2
  * ASG
  * ELB
  * RDS
* Beanstalk is free but you pay for the underlying instances
* Beanstalk use `CloudFormation` under the hood
* remove older versios that are not used using `Life Cycle Policy` so that new versions can be created by your applications

* 3 Architecture models:
  * Single Instance Deployment: good for dev
  * LB + ASG: greate for production or pre-production web applications
  * ASG only: greate for non-web apps in productions (workers, etc...)

## Properties of ElasticBeanstalk
* 3 components
  * Application
  * Application Version: each deployment gets assigned version
  * Environment Name: dev, test, prod
* You deploy app versions to environments and can promote application versions to the next environment
* Rollback feature to previous application version
* Full control over lifecycle of environments

## Deployment Modes
* Single Instance
  * No Load Balancer
  * environment type is single instance

* High Availability
  * Multiple instances 
  * Includes load balancing and auto scaling group

## Deployment Options for Updates
* All At Once (deploy all in one go)
  * Fastest but instances aren't available to serve traffic for a bit (downtime)

* Rolling
  * Update a few instances at a time (bucket), and then move onto next bucket once the first bucket is healthy
  * No incur extra costs on update 
  * Without downtime

* Rolling with additional batches:
  * Like rolling, but **spins up new instances** to move the batch (so that the old application is still available)
  * Minimal added cost, while maintaining the full capacity to serve our current users in production

* Immutable 
  * spins up new instances in new ASG, deploys version to these intstances and then **swaps all instances when everything is healthy**
  * Allows rollback very quickly, while the current one is untouched and already running at capacity

## Elastic Beanstalk CLI
* `EB cli` makes working with Beanstalk from the CLI easier
* Basic commands are:
  * eb create
  * eb status
  * eb health
  * eb events
  * eb logs
  * eb open 
  * eb deploy
  * eb config
  * eb terminate

## Exam Tips
* Beanstalk with HTTPS
  * load the SSL cert onto the load balancer
  * Can be done from the console (EB console, load balancer configuration)
  * Can be done from the code: 
  `ebextension/securelistener-alb.config`
  * SSL cert can be provisioned using ACM (AWS Cert Manager) or CLI
  * Must configure a security group rule to allow incoming 443 (HTTPS port)

## Beanstalk redirect HTTP to HTTPS
  * Configure your instances to redirect HTTP to HTTPS
  * Or configure the ALB with rule
  * Make sure health checks are not redirected (so they keep giving 200 OK)
  * Create an `.ebextension/elb.config` file to configure the Loaad Balancer


## Web Server vs Worker Environment
* Worker Environment 
  * performs tasks that are long to complet, offload these tasks to a dedicated environment
  * Decompling you application into two tiers is common
  * e.g. processing video, generating zip,
  * Define periodic tasks in a cron.yaml

* Web Server
  * Run a website, web application or web API that serves HTTP requests

## RDS with Beanstalk
* Steps to migrate from RDS coupled in EB to standalone RDS:
  * Take an RDS DB snapshot
  * Enable deletion protection in RDS
  * Create new environment without an RDS, point to existing old RDS
  * Perform blue/green deployment and swap new and old environments
  * Terminate the old environment
  * Delete Cloudformation stack

## ElastiCache with Beanstalk
* Create an `elasticache.config` in the `.ebextensions` folder which is at the root of the code zip file and provide appropiate configuration

## Improve performance of Beanstalk with dependencies
* Resolve the dependencies beforehand and package them in the zip file uploaded to ElasticBeanstalk