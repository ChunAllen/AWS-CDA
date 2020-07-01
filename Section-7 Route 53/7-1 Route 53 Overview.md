# Route 53
* Is managed DNS (Domain Name System)
* Is a collection of rules and records which helps clients understand how to reach a server through URLs
* Prefer Alias over CNAME for AWS resources for performance reasons
* TTL is mandatory for each DNS record

## Types of Records:
* A - URL to IPv4
* AAAA - URL to IPv6
* CNAME - URL to URL
* Alias - URL to AWS Resource (e.g. ALB)


## Routing Policies
* **Simple Routing Policy**
  * Use when you need to redirect to a single resource
  * If multiple values are returned, a random one is chosen by the client

* **Weighted Routing Policy**
  * Control the % of the requests that go to specific endpoints

* **Latency Routing Policy**
  * Redirect to the server that has the least latency close to us
  * Latenncy is evaluated in terms of user to designated to AWS Region
  * Nearest to the user

* **Failover Routing Policy**
  * If failed to access the primary it will be redirected to a secondary endpoint 
  * Good for disaster recovery

* **Geo Location Routing Policy** 
  * This is routing based on user location
  * We specify traffic from UK should go to specific IP

* **Multi Value Routing Policy**
  * Use when routing traffic to multiple resources
  * want to associate a Route 53 health checks with records
