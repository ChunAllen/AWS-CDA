# CloudFormation
* Is a declarative way of outlining your AWS Infra

## How CloudFormation Works
* templates have to be uploaded in S3 and referenced in CloudFormation
* To update a template, we **cannot** edit previous ones. We have to upload a new version of the template to AWS
* Stacks are identified by name 
* Deleting a stack deletes every single artifact that was created by CloudFormation

## Deploying CloudFormation Template
* Manual Way
  * Editing templates in CloudFormation designer
  * Using the console to input params

* Automated Way
  * Editing templates in YAML file
  * Using the AWS CLI to deploy the templates
  * Recommended way when you fully want to automate your flow

## CloudFormation Building Blocks
1) Resources - AWS resources declared in template (Mandatory) e.g. S3, EC2
2) Parameters - dynamic inputs for your template
3) Mapping - static variables for your template
4) Output - references to what has been created
5) Conditionals - List of conditions to perform resource creation
6) Metadata

Note:
  * Template Helpers:
    * References
    * Function


## Sampe CloudFormation
Requirements:
  * Create an EC2 instance
  * Create Elastic IP to it
  * Add two security groups 

CloudFormation Template:
```
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-a4ca123
      InstanceType: t2.micro

  ServerSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: !Ref SecurityGroupDescription
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIP: 0.0.0.0/0
      - IpProtocol: tcp
        FromPort: 22
        ToPort: 22
        CidrIP: 192.168.1.1/32
```


## CloudFormation Parameters
* Are way to provide inputs to your CloudFormation template
* They're important to know about if:
  * You want to **reuse** your templates across the company
  * Some inputs cannot be determined ahead of time

## How to reference a parameter
* `Fn::Ref` can be leveraged to reference parameters
* The shorthand for this in YAML is **!Ref**

**Sample Snippet**
```
DbSubnet1:
  Type: AWS::EC2::Subnet
  Properties:
    VpcId: !Ref MyVPC
```

## CloudFormat Mappings
* Are fixed variables within your template
* They're very handy to differentiate between different environments (dev vs prod), regions, AMI types

**Sample:**
```
Mappings:
  RegionMap:
    us-east-1:
      "32": "ami-123123123"
      "64": "ami-098665078"
    us-west-1:
      "32": "ami-213123923"
      "64": "ami-989721377"
```

## Mappings vs Parameters
* Mappings:
  * Greate when you know in advanced all the values can be taken and when they can be deducted from variables such as 
    * Region
    * AZ
    * AWS Account
    * Environment
  * Safer control over the template

* Parameters
  * Parameters when the values are really user specific

## Fn::FindInMap
Note: Accessing Mapping Values
* We use `Fn::FindInMap` to return a named value from specific key
* `!FindInMap [MapName, TopLevelKey, SecondLevelKey]`

**Sample**
```
Mappings:
  RegionMap:
    us-east-1:
      "32": "ami-123123123"
      "64": "ami-098665078"
    us-west-1:
      "32": "ami-213123923"
      "64": "ami-989721377"
Resources:
  MyEC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region, 32], 
```