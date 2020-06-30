# Private vs Public IP (IPv4)

### Public IP:
* means the machine can be identified on the internet (www)
* must be unique across the whole web 
* Can be geo-located easily

### Private IP
* means the machine can only be identified on a private network only
* must be unique accross the private network
* machines connect to WWW using a NAT + internet gateway (a proxy)

### Elasic IP
* When you restart EC2 instance the public IP will change automatically. If you need to have a fixed public IP you need an elastic IP
* Can only be attached to one instance at a time
* You can only have 5 elastic IP per account

Note:
* Avoid using Elastic IP
* They often reflect poor architecutural decisions
* Instead use a random public IP and register a DNS name to it