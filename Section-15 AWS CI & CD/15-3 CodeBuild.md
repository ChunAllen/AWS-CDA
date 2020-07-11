# CodeBuild

* Full managed build service
* Alternatice to other build tools such as Jenkins
* Leverages Docker under the hood for reproducible builds
* Integration KMS for encryption of build artifacts
* Build instructiion can be defined in code 
**buildspec.yml**
* Output logs to S3 and CloudWatch logs
* Metrics to monitor Codebuild stats

## CodeBuild BuildSpec
* buildspec.yml must be at the root of your code
* Define environment variables:
  * plaintext variables
  * secure secrets: use SSM Parameter store

* Phases
  * Install - install dependencies you may need for your build
  * Pre build - final commands to execute before build 
  * Build - actual build commands
  * Post Build - finishing touches (zip output for example)
  
* Artifacts - what to upload to s3 (encrypted with KMS)

Note:
  * CodeBuild containers are deleted at the end of execution (success or failed), you cannot SSH into them, even when they're running
  * CodeBuild can run any commands, so that you can use it to run commands including generating static website and copy your static web files to AWS S3