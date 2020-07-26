## AWS API Gateway Part 2

## API Gateway Swagger / Open API Spec
* Import existing Swagger / Open API 3.0 spect to API Gateway
  * Method
  * Method Request
  * Integration Request
  * Method Response
  * + AWS extentsions for API Gateqway and setup every single option

* Can export current API as Swagger / OpenAPI Spec
* Swagger can be written in YAML or JSON
* Using Swagger we can generate SDK for our applications

## Caching API Responses
* Default TTL is 300 seconds (min: 0s, max: 3600s)
* Cache are defined per stage
* Cache encryption option
* Cache capacity between 0.5GB to 237GB
* Possible to override cache settings for specific methods
* Able to flush the enture cache
* Clients can invalidate the cache with header: **Cache-Control:max-age=0** 

## Logging, Monitoring, Tracing
* CloudWatch Logs
  * Enable CloudWatch logging at the Stage level
  * Can override settings on a per API basis
  * Logs contains information about request / response body

* CloudWatch Metrics
  * Metrics are by stage
  * Possibility to enable detailed metrics

* X-Ray:
  * Enable tracing to get extra information about request in API Gateway

  Note: To enable monitoring, you need to go to Settings, then provide CloudWatch Log ARN provided from IAM role of API Gateway with CloudWatch

  ## AWS CORS
  * CORS must be enabled when you receive API calls from another domain
  * The OPTIONS pre-flight request must contain the following headers:
    * Access-Control-Allow-Methods
    * Access-Control-Allow-Headers
    * Access-Control-Allow-Origin
  * CORS can be enabled through console

  ## Usage Plands & API Keys
  * Usage Plans:
    * Throttling set overall capacity and burst capacity
    * Quotas: number of request made per day / week / month
    * Associate with desired API Stages

  * API Keys:
    * Generate one per customer
    * Associate with usage plans
  
  * Ability to track usage for API Keys

## API Gateway Security
* IAM Permissions
  * API Gateway verifies IAM permissions passed by calling the application
  * Good to provide access within your own infrastructure
  * Leverages **Siv v4** capability where IAM credential are in headers
  * If you saw `sig v4` it means IAM permission

* Lambda Authorizer / Custom Authorizer
  * Uses AWS lambda to validate the token in header being passed
  * Option to cache result of authentication
  * Helps to use OAuth / SAML / 3rd party type of authentication
  * Lambda must return an IAM policy for the User

* Cognito User Pools
  * Cognito fully manages user lifecycle
  * API Gateway verifies identity automatically from AWS Cognito
  * No custom implementation required
  * Cognito only helps with authentication, not authorisation
  * You manage your own user pool, can be backed by Facebook, Google login, ewtc..
