# AWS API Gateway

* No infrastructure to manage
* Handle API versioning (v1, v2...)
* Handle different environments (dev, test, prod...)
* Handle security (Authentication, Authorization)
* Create API Keys, Handle request throttling 
* Swagger / Open API import to quickly define APIs
* Cache API responses

## API Gateway Integrations 
* Outside of VPC
  * Lambda
  * Endpoints on EC2
  * Load Balancers
  * Any AWS Service
  * External and publicly accessible HTTP endpoints

* Inside of VPC
  * Lambda in your VPC
  * EC2 endpoints in VPC

## Notes:
* if the endpoint type is
    * Regional - it will be available for clients in same region 
    * Private - can only be accessed from Amazon VPC
    * Edge Optimized - best for geographically distributed VPC
* See more: https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-endpoint-types.html

* Method - HTTP Verbs (Get, Put, Head, Options, Post, Patch, Any, Delete
    * can create multiple under an API or resource
* Integration Type - tied up to a method upon creation
    * HTTP, Lambda Function, Mock, AWS Service, VPC Link
* Resource - endpoint to your API or path to an API
    * e.g. /users, /products

## Deployment Stages
* You need to make a **Deployment** for them to be in effect
* Changes are deployed to **Stages** 
* Use the naming you like for stages (dev, test, prod)
* Each stage has its own configuration parameters
* Stages can be rolled back as a history of deployment skept

## Stage Variables
* In order to redirect my API Gateway stage to the correct AWS Lambda alias
* are like environment variables for API Gateway
* Use them to change often changing configuration values
* Stage variables are passed to the **context** object in AWS Lambda
* They can be used in:
  * Lambda function ARN
  * HTTP Endpoint
  * Parameter mapping templates

* Use cases:
  * Confugre HTTP endpoints your stages talk to (dev, test, prod...)
  * Pass configuration params to Lambda throught mapping templates

## Lambda Aliases
* For e.g. Stage Variable `prod` is equal to the Alias `PROD`

## Canary Deployment 
* Possiblity to enable canary deployments for any stage
* Choose the % of traffic the canary channel receives
* Metrics & Logs are separate 
* Possibility to override stage variables for canary
* The is blue / green deployment with AWS Lambda & Gateway
* Blue Green Deployment - Testing Two versions at the same time gradually before completely move to the latest version

## Mapping Templates 
* Mapping templates can be used to modify request / responses
* Rename params
* Modify body content
* Add Headers
* Map JSON to XML for sending to backend or back to client
* Uses Velocity Template Language (VTL): for loop, if, etc...
* Filter output results

Note: You can chang ethe mapping template under **Integration Response**