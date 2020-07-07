# AWS CLI Fundamentals

## Accessing S3 using AWS CLI
* **aws s3 ls** - List all buckets
* Download a file from a bucket
```
aws s3 cp s3://<bucket-name>/<file-name> <local-file-name>
```
* **aws s3 mb s3://<bucket-name>** - Create bucket 
* **aws s3 rb s3://<bucket-name>** - Delete bucket

## Initialize an EC2 instance
```
$ aws ec2 run-instances --image-id <ami-of-os> --instance-type <size>
ex. aws ec2 run-instances --image-id ami-048a01c78f7bae4aa --instance-type t2.micro
```

## Adding IAM role to an EC2 Correct way
1. Once you ssh to the EC2 server check if the aws-cli is installed by running aws --version otherwise install aws-cli
2. Run aws configure and leave the AWS key and secret as empty and add the region ap-southeast-1
3. Once install create a new roles in IAM select AWS Service and choose EC2 for the service 
4. Add you desired permission. For example AmazonS3ReadOnlyAccess
5. Right click the created instance and go to Instance Settings > Attach/Replace IAM Role and apply role
6. Now you can test by running aws s3 ls

## IAM Roles & Policies
* In IAM you can create
  * Policies 
  * Roles and attach inline policies

* You cannot attach EC2 IAM roles to on premise servers

* To teest wheather your policy is working you can use
  * IAM Policy Simulator or Dry Run CLI option

* Run **aws configure** to configure credentials on your local machine

* For e.g. **AmazonS3ReadOnlyAccess** policy contains
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:Get*",
        "s3:List*",
      ],
      "Resource": "*"
    }
  ]
}
```

## Getting MetaData of EC2 instance
* ssh to ec2 instance
* curl http://169.254.169.254/latest/meta-data
* The CLI uses the metadata service to get temporary credentials 

## AWS CLI STS Decode Errors
* Use STS (Security Token Service) to decode error messages
* Run command using 
```
aws sts decode-authorization-message --encoded-message <message to decode>
```

## AWS SDK Overview
* It's recommended to use the **default credential provider chain**
* If you don't specify or configure a default region, then us-east-1 will be chosen by default
* The priority in the CLI credentials chain is
```
Command Line Options > Environment Variables > EC2 instance profile
```

## AWS CLI Profiles
* This is to handle multiple aws credentials on your local machine
* By default if you run an aws command it will use the default credentials
* To check the aws credentials go to cd ~/.aws/ then cat credentials
* To create another aws profile use aws configure --profile my-other-aws-account
* To use a specific profile in aws cli use aws s3 ls --profile my-other-aws-account