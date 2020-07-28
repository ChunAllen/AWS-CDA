# SSM Parameter Store
* Secure storage for configuration and secrets
* Notifications with CloudWatch Events
* Integration with CloudFormation

## SSM Parameter Store Hierarchy
```
/my-department/
  my-app/
    dev/
      db-url
      db-password
    prod/
      db-url
      db-password
```

## Parameter Policies (for advanced parameters)
* Allow to assign a TTL to a parameter (experation date) to force updating or deleting sensitive data such as passwords
* Can assign multiple policies at a time

## SSM Param Store Types
* String 
* StringList - seperate strings using commas
* SecureString - encrypt senstivie data using the KMS for your account

## API CLI
* Getting Parameters
```
aws ssm get-parameters --names /my-app-/dev/db-url 
/my-app/dev/db-password
```

* Getting Decrypted Params
```
aws ssm get-parameters --names /my-app-/dev/db-url 
/my-app/dev/db-password --with-decryption
``` 

* To get all params
```
aws ssm get-parameters-by-path --path /my-app/dev/
```

# AWS Secrets Manager
* Newer service, meant for storing secrets
* Capability to force rotation of secrets every X Days
* Automate generation of secrets using Lambda
* Ingeration with RDS
* Secrets are encrypted using KMS

## SSM Param Store vs Secrets Manager
* Secrets Manager
  * Automatic rotation of secrets with AWS Lambda
  * Integration with RDS
  * KMS encryption is mandatory
  * Can integrate with CloudFormation

* SSM Param Store
  * Simple API
  * No Secret rotation
  * KMS is optional
  * Can integrate with CloudFormation
  * Can pull a secrets manager secret usng the SSM param store API
  * We would like to audit the values of an encryption value over time

## CloudWatch Logs - Encryption
* Encryption is enabled at the log group level, by associating a CMK with a log group
* You cannot associate a CMK with a log group using the CloudWatch console
* You must use the CloudWatch Logs API
  * `associate-kms-key` - if the log group already exists
  * `create-log-group` - if the log group doesn't exist yet

# note
* An EC2 instance is trying to download a file from S3 that is encrypted with SSE:KMS. It's getting a denied exception, even though the IAM policy allows access to that S3 object. What do you recommend?

  * Anser: Add permission KMS:Decrypt