# CloudFormation Part 2

## Outputs
* Declares **optional** values that we can import into other stacks
* They're very useful for example if you want to defined a network CloudFormation and output the variables as VPC ID and your Subnet IDs
* it's the best way to perform some collaboration cross stack
* You cannot delete CloudFormation Stack if its outputs are being referenced by another stack

**Sample**
```
Outputs:
  StackSSHSecurityGroup:
    Description: The SSH Security Grouo for our Company
    Value: !Ref MyCompanySSHWideSecurityGroup
    Export:
      Name: SSHSecurityGroup
```