# AWS Cognito

* Cognito User Pools
  * Sign in functionality for app users
  * Integrate with API Gateway
  * Create a serverless database of user for your mobile apps
  * Simple login: Username or Email / password combination
  * Possiblity to verify emails / phone numbers and add MFA
  * Sends back a JSON Web Tokens (JWT)
  * Can be integrated with API Gateway for authentication
  * provide a Facebook login before your users call your API hosted by API Gateway. You need seamlessly authentication integration, you will use


* Cognito Identity Pools (Federal Identity):
  * Provide AWS credentials to users so they can access AWS resources directly
  * Integrate with Cognito User Pools as an identity provider

* Cognito Sync
  * Synchronize data **offline** between mobile devices
  * May be deprecated and replaced by AppSync
  * Use for mobile platforms, iOS, Android
  * Requires Fedetal Identity Pool in Cognito (not user pool)

* Federation User Pools 
  * Allows you to login using 3rd party provider e.g. Facebook, Google, Etc. 
  

* Identity Pools (Federal Identities)
  * To provide user access to AWS **temporarilty**

* App Client - will be created under User Pools, this will give unique ID & optional secret key to access user pool
  * allows you to add callback url in case the sign in is success
  * congnito user pools can have multiple App Clients since you can assign 1 callback url per AppClient, 
  * app clients will share the db of users in cognito user pools


