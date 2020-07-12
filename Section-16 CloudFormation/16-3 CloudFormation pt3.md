# CloudFormation Part 3

## ChangeSets
* When you update a stack you need to know what changes before it happens for greater confidence
* `ChangeSets` won't say if the update will be successful


## Nested Stacks
* are stacks as part of other stacks
* allow you to isolate repeated patterms / common components in separate stacks and call them from other stacks
* Example:
  * Load Balancer configuration that is re-used
  * Security Group that is re-used
* Considered as best practice
* To update a nested stack, always update the parent

## Cross Stacks
* Helpful when stacks have different lifecycles
* Use outputs export and `Fn::ImportValue`
* When you need to pass export values to many stacks (VPC Id, etc..)

## StackSets
* Create, update, or delete stacks across **multiple accounts and regions** with a single operation