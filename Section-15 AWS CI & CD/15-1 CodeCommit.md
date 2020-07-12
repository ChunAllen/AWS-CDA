# AWS CI / CD 

## Continuous Integration
* CodeCommit - Developers push code to a repository 
* CodeBuild - testing / build server checks the code as soon as its pushed e.g. Jenkins

Note: Developers get feedback about the test and checks that have passed / failed

## Continous Delivery
* Ensure that the software can be released reliably whenever needed
* CodeDeploy - automated deployment

## Technology Stack fo CI/CD
* Code - CodeCommit
* Build & Test - CodeBuild
* Deploy & Provision - Beanstalk & CodeDeploy
* Orchestrate everything - CodePipeline

## CodeCommit
- version control is the ability to understand the various changes that happened to the code over time

Note:
  * It does not support HTTP public access
  * Only SSH, HTTPS, IAM

* Security
  * Authentication in Git
    * SSH Keys
    * HTTPS
    * MFA
  
  * Authorization in Git
    * IAM Polices manage over / roles rights to repositories
  
  * Encryption
    * Repositories are automatically encrypted at rest using KMS
    * Encrypted in transit (can only use HTTPS or SSH)

* Notifications
  * Can trigger SNS, Lambda, or CloudWatch Event Rules

  * Use Cases for SNS / Lambda
    * deletion of branches
    * trigger for pushes that happens in master branch
    * notify external build system
    * trigger aws lambda function to perform codebase anaylysis

  * Use cases for CloudWatch Event Rules:
    * Trigger for pull request updates
    * Commit comment events
    * CloudWatch Event Rules goes into an SNS Topic
    * Send email alerts e.g. anytime pull request is open or comments added to CodeCommit