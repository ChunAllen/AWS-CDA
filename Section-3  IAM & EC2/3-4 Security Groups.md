### Security Groups

* acting as a "Firewall" on EC2 instances
* Access to Ports
* Authorised IP ranges - IPv4 and IPv6
* Control of inbound network (From other instance)
* Control of outbound network (From the instance to other)


### Good to know
* Can be attached to multiple instances
* Locked down to a region / VPC Combination
* It's good to maintain one separate security group for SSH Access
* If your application gives "connection refused" then it's a security group issue
* All inbound traffic is blocked by default
* All outbound trraffic is authorised by default