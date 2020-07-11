# CodePipeline

* Continous delivery
* Visual Workflow
* Source: GIthub / CodeCommit / Amazon s3
* Build: CodeBuild / Jenkins / etc..
* Deploy: CodeDeploy / Beanstalk / CloudFormation / ECS

## CodePipeline Troubleshooting
* Each pipeline stage can create artifacts
* Artifacts are passed stored in S3 and passed on to the next stage
* CodePipeline state changes happen in CloudWatch Events which can return create SNS notifications:
  * you can create events for failed pipelines
  * you can create events for cancelled stages

* CloudTrail can be used to audit API Calls
* Pipeline can't perform an action, make sure the IAM Service Role attached does have enough permissions

Notes:
* AWS Code Pipeline Generates artifacts 
* It has stages like Pull from CodeCommit when there are any changes from the branch then deploy to staging
* Stages can have multiple actions. For example: Manual Approval before deploying to production 