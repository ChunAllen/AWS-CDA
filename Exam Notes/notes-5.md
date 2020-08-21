# Exam Notes Part 5

### To guarantee ordering of a message in SQS FIFO queue the attribute should belong to
* MessageGroupID - is the tag that specifies that a message belongs to a specific message group. Messages that belong to the same message group are always processed one by one, in a strict order relative to the message group (however, messages that belong to different message groups might be processed out of order).

**Other Comparison:**
* MessageDeduplicationID - is the token used for the deduplication of sent messages. If a message with a particular message deduplication ID is sent successfully, any messages sent with the same message deduplication ID are accepted successfully but aren't delivered during the 5-minute deduplication interval.

### To enable EC2 instances to run in multiple docker containers simultaneously
* Docker Multi-Container Platform - allows you to define your software stack and store it in an image that can be downloaded from a remote repository. Use the Multicontainer Docker platform if you need to run multiple containers on each instance. The Multicontainer Docker platform does not include a proxy server. Elastic Beanstalk uses Amazon Elastic Container Service (Amazon ECS) to coordinate container deployments to multi-container Docker environments.

**Other Comparison:**
* Docker Single-Container Platform - allows you to define your software stack and store it in an image that can be downloaded from a remote repository. Use the Single Container Docker platform if you only need to run a **single** Docker container on each instance in your environment. The single container platform includes an Nginx proxy server.

* Custom Platform - provides more advanced customization than a custom image in several ways. A custom platform lets you develop an entirely new platform from scratch, customizing the operating system, additional software, and scripts that Elastic Beanstalk runs on platform instances.  
