# AWS CloudFront

* Content Deliver Network (CDN)
* Improves performance, content is cached at the edge
* 216 point of presence globally (edge locations)
* DDos protection, integration with Shield, AWS Web Application Firewall
* Can expose external HTTPS and can talk to internal HTTPS backends

* S3 Cross Region Replication (CRR) - allows you to replicate the data from one bucket in a region to another bucket in another region

## Exmaple Use Case:
* User is reading from s3 bucket in Australia
* But instead of reading directly from AU user will read from the cache edge network nearest to his/her location
* The cache in the US was from AU
* If the data is not in cache of edge location it will get directly from s3
* but if the data is cached in edge location it will get the cache from it

## Cloudfront - Origins
* S3 Bucket
  * For distributing files and caching them at the edge
  * Enhanced security with CloudFront Origin Access Identity (OAI)
  * CloudFront can be used as an ingress (to upload files to s3)

* Custom Origin (HTTP)
  * Application Load Balancer
  * EC2 instance
  * S3 website
  * Any HTTP backend you want

NOTE:
  * To get data from Origin the edges will have Origin Access Identity to validate access

## CloudFront Geo Restriction
* You can restrict who can access your distribution
  * Whitelist - allow your uses to access your content only if they're in one of the countries on a list approved countires

  * Blacklist - prevents your users from access your content if they're in one of the countries on a blacklist of banned countries

* A "country" is determined using 3rd party Geo-IP database

## CloudFront and HTTPS
* Viewer Protocol Policy 
  * Redirect HTTP to HTTPS 
  * Or use HTTPS only

* Origin Protocol Policy (HTTP or s3):
  * HTTPS only
  * Or MatchViewer
  (HTTP => HTTP & HTTPS => HTTPS)

Note:
  * S3 bucket "websites" don't support HTTPS

## CloudFront Signed URL / Signed Cookies
* If you want to distribute paid shared content to premium users over the world
* Policies attached to Signed URL
  * URL expiration
  * IP ranges to access data from
  * Trusted Signers (which AWS accounts can create signed URLs)
* Signed URL = access to individual files (one signed URL per file)
* Signed Cookiees = access to multiple files (one signed cookie for many files)

## CloudFront Signed URL vs S3 Pre-Signed URL

* CloudFront Signed URL
  * Allow access to a path, no matter the origin
  * Account wide key-paid only the root can manage it
  * Can filter by IP, path, date, expiration
  * Can leverage caching features

* S3 Pre-Signed URL
  * Issue a request as the person who pre-signed the URL
  * Uses the IAM key of signing IAM principal
  * Limited lifetime