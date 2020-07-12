# CloudFormation Part 2

## Outputs
* Declares **optional** values that we can import into other stacks
* They're very useful for example if you want to defined a network CloudFormation and output the variables as VPC ID and your Subnet IDs
* it's the best way to perform some collaboration cross stack
* You cannot delete CloudFormation Stack if its outputs are being referenced by another stack
* Exported Output names **must be** unique within your region

**Sample**
```
Outputs:
  StackSSHSecurityGroup:
    Description: The SSH Security Grouo for our Company
    Value: !Ref MyCompanySSHWideSecurityGroup
    Export:
      Name: SSHSecurityGroup
```

## Cross Stack Reference
* We then create a second template that leverages that security group
* For this we use the `Fn::ImportValue` function
* You can't delete the underlying stack until all references are deleted too

```
Resources:
  MySecureInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-123asd1
      InstanceType: t2.micro
      SecurityGroups:
        - !ImportValue SSHSecurityGroup
```

### Conditions
* Conditions are used to control the creation of resources or outputs based on a condition
* Common conditions:
  * Environment (dev/test/prod)
  * AWS Region
  * Any Parameter Value

```
Conditions:
  CreateProdResources: !Equals [!Ref EnvType, prod]
```
Note: If `EnvType` is equals to `Production` then returns `prod`

* The intrinsic function (logical) can be any of the following:
  * Fn::And
  * Fn::Equals
  * Fn::If
  * Fn::Not
  * Fn::Or

## Must Know
* Ref
* Fn::GetAtt
* Fn::FindInMap
* Fn::ImportValue
* Fn::Join
* Fn::Sub
* Condition Functions (Fn::If, Fn::Not, Fn::Equals, etc...)

## Fn::Ref
* Can be leveraged to reference
  * Parameters: returns the value of the parameter
  * Resources: returns the physical ID of the underlying resource e.g. EC2 ID
* Shorthand for this is `!Ref`
* It **cannot** be used to reference `Condition`

```
DbSubnet1:
  Type: AWS::EC2::Subnet
  Properties:
    VpcId: !Ref MyVPC
```

## Fn::GetAtt
* Attributes are attached to any resources you create
* To know the attributes of your resources, the best place to look at is the documentation
* For example: The AZ of an EC2 Machine

```
NewVolume:
  Type: "AWS::EC2::Volume"
  Condition: CreateProdResources
  Properties:
    Size: 100
    AvailabilityZone:
      !GetAtt EC2Instance.AvailabilityZone
```

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

## Fn::ImportValue
* Import values that are exported in other templates
```
Resources:
  MySecureInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-1asdas1
      InstanceType: t2.micro
      SecurityGroups:
        - !ImportValue SSHSecurityGroups
```

## Fn::Join
* Join values with delimiter
```
!Join[delimiter, [comma-delimited list of values]]
```

**Sample**
```
!Join[":", [a,b,c,]]
```
this returns `a:b:c`

**Sample**
```
Fn::Join:
  - ''
  - [IPAddress=, !Ref 'IPAddress']
```
this returns `IPAddress=10.0.0.1`

## Fn::Sub
* shorthand for !Sub, is used to substitute variables from text.
* String must containe `${VariableName}` and will substiture them

```
!Sub
  - String
  - { Var1Name: Var1Value, Var2Name: Var2Value }
```

```
!Sub String
```