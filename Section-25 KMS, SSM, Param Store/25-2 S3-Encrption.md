# S3 Encryption

* There are 4 methods of encrypting objects in S3
  * SSE-S3 - encrypts S3 object using keys handled and managed by AWS
  * SSE-KMS - leverages KMS to manage encryption keys
  * SSE-C - when you want to manage your own encryption keys
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
