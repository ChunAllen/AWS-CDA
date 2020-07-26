# AWS Step Functions

* Build serverless visual workflow to orchestrate your lambda functions
* Represent flow as JSON state machine
* Features:
  * Sequence
  * Parallel - You need to orchestrate multiple Lambda functions and wait for the result of all of them before making a final computation. What do you recommend?
  * Conditions
  * Timeouts
  * Error Handling
* Maximum execution time of 1 year
* Possibility to implement human approval feature

## Error Handling
* By default, when a state reports and error, AWS Step Functions causes the execution to **fail entirely**
* Retriying Failures - Retry: IntervalSeconds, MaxAttempts, BackoffRate
* Moving on - Catch: ErrorEquals, Next

## Standard vs Express
* Standard
  * Duration 1 year
  * Execution start rate: 2,000 per second

* Express
  * Duration 5 minutes
  * Over 100,000 per second

## Sample Code:
```
{
  "Comment": "A Helllo World example demonrating various state tyoes of the AWS State Language",
  "StartAt": "Pass",
  "States": {
    "Pass": {
      "Comment": "A pass state passes its input to its output",
      "Type": "Pass",
      "Next": "Hello World example?"
    },
    "Hello World example?": {
      "Comment": "A choice state adds branching logic to a state machine",
      "Type": "Choice",
      "Choices": [
        {...}        
      ]      
    }
  }
}
```