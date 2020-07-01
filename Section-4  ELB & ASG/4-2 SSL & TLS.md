# SSL / TLS Basics

* Allows traffic to be encrypted (in-flight encryption)
* Refers to Secure Sockets Layer
* Tranport Layer Security (TLS) - newer version 

## Load Balancer - SSL Certs
* SSL certs can be managed using ACM (AWS Certificate Manager)

## SSL - Server Name Indication
* solves the problem of loading multiple SSL certs onto one web server (to serve multiple websites)
* It's newer protocol and requires the client to indicate the hostname of the target server in the initial SSL handshake

## Elasitc Load Balancers - SSL Certificates
* Classic Load Balancer 
  * Supports only one SSL cert
  * Must use multiple CLB for multiple hostname with multiple SSL Certs

* Application Load Balancer (v2) and Network Load Balancer (v2)
  * Supports multiple listener with multiple SSL certs
  * Uses server name indication (SNI) to make it work