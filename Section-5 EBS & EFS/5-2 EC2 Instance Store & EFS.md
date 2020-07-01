# Instance Store

* Some instances do not come with root EBS volumes instead they come with Instance Store (= ephemeral storage)
* Instance Store is physically attached to the machine (EBS is a network drive)
* Pros:
  * Better I/O performance
  * Good for buffer / cache / scratch data / temporary content
  * Data survives reboots
* Cons 
  * On stop or termination, the instance store is lost
  * You can't resize the instance store
  * Backups must be operated by the user
  

  # EFS - Elastic File System
  * Use cases: content management, web serving, data sharing, Wordpress
  * Compatible with Linux based AMI (now windows)
  * Encryption at rest using KMS

  # EBS vs EFS vs Instance Store
  * EBS is a network drive, locked in AZ
  * EFS shares website files (Wordpress), accessible across AZ
  * Instance Store is a physical drive, high performance but possible to lose data
  