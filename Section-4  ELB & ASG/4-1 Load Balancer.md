# Load Balancer 
* Are servers that forward internet traffic to multiple serves (EC2) downstreams
* frontline for the user and their request will go thru different server.
* Provides static DNS name that we can use in our application.

## Why use load balancer?
* Spread load across multiple downstream instances
* Expose single point of access (DNS) to your application
* Seamlessly handle failures of downstream instances
* Do regular health checks to your instances
* Provide SSL termination (HTTPS) for your websites
* Enforce stickiness with cookies
* Highly available across zones
* Separate public traffic from private traffic
* You can setup internal (private) or external (public) ELBs
* It supports Health Checks to check if the Load balancer is active or not

## Types of Load Balancer
* Classic Load Balancer - v1 old generation 2009 (Deperated)
* Application Load Balancer - v2 new generation 2016
  * balancing to multiple HTTP applications across machines (target groups)
  * balancing to multiple application on same machine e.g. containers
  * balancer based on hostname in URL
  * Great for micro services & container based application (Docker & Amazon ECS)
  * Has a port mapping feature to redirect to dynamic port
  * Stickiness can be enable at target group level 
  * The application server dont see the IP of the client directly 
  * True IP of the client is inserted in the header X-Forwared-For

* Network Load Balancer - v2 new generation 2017
  * mostly used for extreme performance and should not be the default load balancer you choose


Note:
* ALB for HTTP / HTTPs and Websocket
* NLB for TCP
* CLB and ALB supports SSL certificates and provide SSL Termination
* All Load Balancers have health check capibility
* ALB can route on based on hostname /path
* ALB is grate fit with ECS

# Cross- Zone Load Balancing

* Each load balacner instance distributes evently across all registered instance in all AZ
* Cross Zone Load Balancer is in load balancer level it is disabled by default