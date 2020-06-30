# Elastic Network Interfaces (ENI)

* Logical component in a VPC that represents a network card
* You can create ENI independently and attach them on the fly (move them) on EC2 instances for failover
* Bound to a specific availability zone 
* By default EC2 instance have only 1 private IP, if you want to have a primary and secondary IP for EC2 you need to attach ENI