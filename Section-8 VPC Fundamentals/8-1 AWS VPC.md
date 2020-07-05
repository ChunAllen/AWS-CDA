# AWS VPC

* Within a region, you're ablt to create VPC  (Virtual Private Cloud)
* Each VPC contains subnets (networks)
* Each subnet must be mapped to an AZ
* Public Subnet (Public IP)
* Private Subnet (Private IP)

## Public Subnets vs Private Subnets
* Public usually contains
  * Load Balancers
  * Static Websites
  * Files
  * Public Authentication Layers
  * It is accessible from the internet

* Private usually contains
  * Web application servers
  * Databases
  * It should not be accessible from the internet

**Note** Public and private subnets can communicate if they're in the same VPC

## Internet Gateways and NAT Gateways
* Internet Gateway (IG)
  * IG helps our VPC instances connect with the internet
  * Public Subnets have a route to internet gateway

* NAT Gateways
  * allow your instances in your private subnets to access the internet while remaining private

## Network ACL & Security Groups
  * NACL 
    * firewalle which controls traffic from and to subnet
    * Can have ALLOW and DENY rules
    * Are attached at the subnet level

  * Security Groups
    * A firewall that controls traffic from an ENI / EC2 instance
    * Can only have ALLOW rules
    

## VPC Flow Logs
  * Capture information about IP traffic going into your interfaces
  * Monitors the traffic in Subnet and Security Group

## VPC Peering 
  * Connect two VPC **privately** using AWS Network

## VPC Endpoints
  * allows you to connect AWS services using a private network instead of internet network

## Site to Site VPN and Direct Connect
  * STS VPN
    * Connect an on-premise VPN to AWS
    * Goes over the public internet
  
  * Direct Connect (DX)
    * Establish a **physical connection** between on-premises and AWS
    * Takes at least a month to establish

  ## VPC Brain Dump
  * VPC - virtual private cloud
  * Subnets - tied to an AZ, network partition of the VPC
  * Internet Gateway - at the VPC level, provide internet access
  * NAT Gateway / Instances - give internet access to private subnets
  * NACL - stateless, subnet rules for an inbound and outbound
  * Security Group - stateful, operate at the EC2 instance level or ENI
  * VPC peering - connect two VPC with non overlapping IP ranges
  * VPC endpoints - provide private access to AWS Services withing VPC
  * VPC Flow Logs - network traffic logs
  * Site to Site VPN - VPN over public internet between on-premisses DC and AWS
  * Direct Connect - direct private connection to AWS physically