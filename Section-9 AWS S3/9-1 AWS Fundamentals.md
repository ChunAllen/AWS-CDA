# AWS S3 Fundamentals

* Buckets allos users to store objects (files) 
* Buckets **should be globally unique**
* Naming convention: 
  * No uppercase, underscore, 3-63 chars long, not an IP, must start in lowercase or number

* Objects are files
* files have key. The key is full path for e.g. bucket/file.txt meaning URL of the file
* Object Values are the content of the body: 
    * Max size 5TB
    * If uploading more than 5TB, must use "multi-part upload"
    * Metadata (list of key / value pairs - system or user metadata)
    * Tags (Unicode key / Value pair up to 10) useful for security / lifecycle
    * Version ID if we enable versioning

# Bucket Properties

## Versioning
  * Enabling versioning allows you to upload same files in the bucket without overwriting it. 
  * For e.g. fileA.txt is already uploaded, then you upload again fileA.txt it will automatically have a version
  * existing files in the bucket will have a null version, when you upload new file it will be version 1

## Encryption for Objects 
* 4 methods of encryption
  * SSE S3 - encrypts S3 objects using keys handled & managed by AWS, objects are encrypted server side

  * SSE-KMS 
    * leverages AWS key management service to manage encryption keys using KMS. Must set header 
  **"x-amz-server-side-encryption":"aws:kms"** 
    * maintains **rotation policy** for the encryption keys

  * SSE-C - when you want to manage your own encryption keys outside of AWS, **HTTPS** must be used  

  * Client Side encryptions 
    * client must encrypt data themeselves before sending to s3
    * If you want the encryption happens in your application before sending to s3

## Security
* User Based - IAM policies which API calls should be allowed for a specific user for IAM console

* Resource Based
  * Bucket policies - bucket wide rules from the S3 Console - allows cross account
  * Object Access Control List (ACL) - finger grain
  * Bucket Access Control List (ACL) - less common

* Networking - supports VPC endpoints (for instances in VPC without www internet)

* Logging and Audit - S3 access logs can be stored in other s3 bucket, API calls can be logged in AWS CloudTrail

* User Security 
  * MFA - can be required in versioned buckets to delete objects
  * Signed URs are urls that are valid for a limited time (e.g. premium video)

* Explicit DENY - will take priority over a bucket policy permission

## S3 Websites

* Allowed you to host static websites and have them accessible on the www
* the website URL will be:
  <bucket-name>.s3-website<AWS-region>.amazonaws.com

* If you get a 403 (forbidden) error, make sure the bucket allows public reads


## S3 CORS
* Cross Oring Resource Sharing - allows you to limit the number of websites taht can request your files (limit your costs)

* If you request data from another S3 bucket, you need to enable CORS

## S3 Consistency Model
* Read after write consistency for PUTS of new objects
  * As soon as an object is written, we can retrieve it e.g. PUT 200 -> GET 200

  * This is true, except if we did a GET before to see if the object existed
  e.g. GET 404 -> PUT 200 -> GET 404 - eventually consistent

* Eventual Consistend for DELETES and PUTS of existing objects

  * If we read an object after updating, we might get the older version

  * If we delete an object, we might still be able to retrieve if for a short time

## Performance (It will show in the exam)
* Faster upload of large objects (>= 100MB), use multipart upload
* Use CloudFront to cache s3 objects around the world (improves read)
* S3 Transfer Acceleration - just need to change the endpoint you write to, not the code
* If using SSE-KMS encrytion, you may be limited to your AWS limits for KMS usage
* when you had > 100TPS (transaction per second), S3 performance could degrade. 
* behind the scene, each object goes to an s3 partition for the best performance, we want the highest permission.
* It is recommended to have random characters in front of your key name to optimise performance
  * e.g. mybucket/53123_my_folder/my_file.txt
* It not recommended to use dates to prefix keys
  * e.g. mybucket/2019_09_09_my_folder/my_file.txt

## NOTES:
When a file is over 100 MB, Multi Part upload is recommended as it will upload many parts in parallel, maximizing the throughput of your bandwidth and also allowing for a smaller part to retry in case that part fails.


