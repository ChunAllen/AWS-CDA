# S3 Encryption

* There are 4 methods of encrypting objects in S3
  * SSE-S3 - encrypts S3 object using keys handled and managed by AWS
           - When you use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3), each object is encrypted with a unique key. As an additional safeguard, it encrypts the key itself with a master key that it regularly rotates. So this option is incorrect.
           
  * SSE-KMS - leverages KMS to manage encryption keys
  * SSE-C - when you want client to their own encryption keys, Server Side Encryption with Customer Manager Keys
  * Client Side Encryption

## SSE-KMS
* KMS advantages user control and audit trail
* Object is encrypted server side
* Must set header `"x-amz-server-side-encryption": "aws:kms"`

## S3 Bucket Policies - Force SSL
* To force SSL, create an S3 bucket policy with a DENY on the condition `aws:SecureTransport = false`
* Using an allow on `aws:SecureTransport = true` would allow anonymous `GetObject ` if using SSL

## S3 Bucket Policies - Force Encryption of SSE-KMS
* Deny incorrect encryption header: make sure it includes `aws:kms (== SSE-KMS)`
* Deny no encryption header to ensure objects are not uploaded un-encrypted
