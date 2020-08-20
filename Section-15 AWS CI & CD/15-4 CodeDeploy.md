# CodeDeploy

* only **allow to deploy** to **EC2** instances
* is a managed service, there are not security groups to manage
* There are several ways to handle deployments using open source tools (Ansible, Terraform, Chef, Puppet, etc...)
* Use **CodeDeploy Deployment Groups** to ensure that the application gets deployed to different sets of EC2 instances at different times allowing for a smooth transition


## Steps to make it work
* Ec2 machine must be running on the CodeDeploy Agent
* the agent continously polling AWS CodeDeploy for work to do
* CodeDeploy sends appspec.yml
* Application is pulled from Github or S3
* Ec2 will run the deployment instructions
* CodeDeploy Agent will report of success / failure deployment on instance

## CodeDeploy AppSpec Hooks
* File Section - how to source and copy from s3 / Github to filesystem
* Hooks are set of instruction to do deploy the new version, the order is:
  * ApplicationStop
  * DownloadBundle
  * BeforeInstall
  * AfterInstall
  * ApplicationStart
  * ValidateService

## Two types of Deployment:
* In Place Deployment 
  * Half at a time
 
* Blue Green Deployment
  * This creates first the v1 and v2 then after everything passed it will delete the v1 and remain the v2 

## CodeDeploy Deployment Groups
* You can specify one or more deployment groups for a CodeDeploy application. The deployment group contains settings and configurations used during the deployment. Most deployment group settings depend on the compute platform used by your application. Some settings, such as rollbacks, triggers, and alarms can be configured for deployment groups for any compute platform.

In an EC2/On-Premises deployment, a deployment group is a set of individual instances targeted for deployment. A deployment group contains individually tagged instances, Amazon EC2 instances in Amazon EC2 Auto Scaling groups, or both.

## CodeDeploy Agent 
* The CodeDeploy agent is a software package that, when installed and configured on an instance, makes it possible for that instance to be used in CodeDeploy deployments. The agent connects the EC2 instances to the CodeDeploy service.

## CodeDeploy Hooks
* Hooks are found in the AppSec file used by AWS CodeDeploy to manage deployment. Hooks correspond to lifecycle events such as ApplicationStart, ApplicationStop, etc. to which you can assign a script.

## CodeStar
* is an integrated solution the regroups Github, CodeCommit, CodeBuild, CodeDeploy, CloudFormation, CloudPipeline, CloudWatch
* Helps create CICD-ready projects for EC2, Lambda, Beanstalk
* Issue Tracking integration with JIRA / GitHub Issues
* One dashboard to view all your components
