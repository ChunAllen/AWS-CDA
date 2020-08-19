# KMS Encryption

## Encryption in Flight (SSL)
* Data is encrypted before sending and decrypted after receiving 
* SSL certificats help with encryption (HTTPS)
* Encryption in flight ensures no MITM (man in the middle attach) can happen
* Store data in encrypted queues
* Encrypting without any changes in code
* Encryption at **rest**

## Encryption Server Side at Rest
* Server side encryption means that the data is sent encrypted to the server first
* Data is encrypted after being received by the server
* Data is decrypted before being sent
* it is stored in an encrypted form thanks to a key (usually a data key)
* The encryption / decryption keys must be managed somewhere and the server must have access to it 

## Client Side Encryption
* Data is encrypted by the client and never decrypted by the server
* Data will be decrypted by a receiving client
* The server should not be able to decrypt the data
* Could leverage Envelope Encryption

# KMS (Key Management Service)
* Easy way to control access to your data, AWS manages keys for us
* Anytime you hear `encryption` for an AWS service, it's most likely KMS
* Fully integrated with IAM for authorization
* Integrated with:
  * Amazon EBS: encrypt volumes
  * S3 - Server side encryption of objects
  * Redshift - encryption of data
  * RDS - encryption of data
  * SSM  - parameter store

## KMS Customer Master Key (CMK) Types 
* Symmetric (AES-256 keys)
  * Services that are integrated with KMS use Symmetric CMKs
  * Necessary for envelope encryption
  * You never get access to the key unencrypted (must call KMS API to use)

* Asymmetric (RSA & ECC key pairs)
  * **Public (Encrypt) and Private (Decrypt)**
  * Used for Encrypt / Decrypt or Sign / Vertify operations
  * The public key is downloadable but you can access the Private key unencrypted
  * Use case: encryption outside of AWS by users who can't call the KMS API


## KMS Needs to Know
* Ably to fully manage keys and policies
  * create
  * rotation policies
  * disable
  * enable
  
* We need to create User Keys in KMS before using the encryption features for EBS, S3, etc... - we can use the aws managed service keys in KMS

* Audit Key usage using CloudTrail
* 3 types of CMK 
  * Managed Service Default CMK - free
  * User Keys created in KMS - $1 per month
  * User keys imported (must be 256-bit symmetric key) - $1 per month
* Never ever store your secrets in plaintext, especially in your code
* Encrypted secrets can be stored in the code / environment variables
* KMS can only help in encrypting up to 4KB of data per call
* If data > 4KB, use envelope encryption
* To give access to KMS to someone 
  * Make sure the Key Policy allows the user
  * Make sure the IAM policy allows the API calls

## KMS key policies
* Control access to KMS keys similar to S3 bucket policies
* Default KMS key policy
  * Created if you don't provide a specific KMS key policy
  * Complete access to the key to the root user = entire AWS account
  * Gives access to the IAM policies to the KMS key

* Custom KMS Key Policy
  * Define users, roles that can access the KMS key
  * Define who can administer the key
  * Useful for cross-account access of your KMS key


  ## Encryption / Decryption using KMS
  * Encryption
  ```
  aws kms encrypt --key-id alias/tutorial --plaintext fileb://ExableSecretFile.txt --output text --query CiphertextBlob --region eu-west-2 > ExampleSecretFileEncrypted.base64
  ```

  * Decryption
  ```
  aws kms decrypt --ciphertext-blob fileb://ExampleSecretFileEncrypted --output text --query Plantext > ExampleFileDecrypted.base64 --region eu-west-2
  ```

## Envelope Encryption
* KMS Encrypt API call has limit of 4KB
* if you want to encrypt > 4Kb you must use **Envelope Encryption**
* The main API that will help us is the `GenerateDataKey` API

Note: For the exam: anything over 4KB data that needs to be encrypted must use the `Envelope Encryption` == `GenerateDataKey` API

## KMS Symmetric - API Summary
* Encrypt - encrypt up to 4KB of data through KMS
* GenerateDataKey - generates a unqiue symmetric data key (DEK)
  * returns a plaintext copy of the data key
  * AND a copy that is encrypted under the CMK that you specify
* GenerateDataKeyWithoutPlaintext
  * Generate a DEK to use at some point
  * DEK that is encrypted under that CMK that you specify (Must use Decrypt later)
* Decrupt - decrypt up to 4KB of data 
* GenerateRandom - returns a random byte string

## KMS Request Quotas
* When you exceed a request quota, you get a `ThrottlingException`
* To respond, use exponential backoff (backoff and retry)
* This includes request made by AWS on your behalf (e.g. SSE-KMS)
* For `GenerateDataKey` consider using DEK caching from the Encryption SDK
* You can request a Request Quotas increase through API or AWS support
