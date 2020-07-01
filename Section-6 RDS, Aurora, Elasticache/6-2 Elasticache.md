# Elasticache 

* Applications queries ElastiCache, if not available to get from RDS and store in ElastiCache
* ElastiCache is to get managed Redis or Memcached
* Caches are in memory databases with really high performance, low latency
* Helps reduce the load of database for read intensive workloads (It will be read from cache instead of database)
* Helps make your application stateless
* Write scaling capability using Sharding
* Read scaling capability using Read Replicas
* Multi AZ with Failover Capability
* AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups
* Helps relieve the load in RDS
* Cache must have an invalidation service
* Read Replicas allow to reduce the read pressure on any of one node
* To make ElastiCache always available enable multi AZ

## Use Cases
* Store application logs like writing session and retrieve session

## Caching Strategies
* **Lazy Loading / Cache-Aside / Lazy Population** 
  * if data is not exist in cache it will read from RDS and write in cachche
  * would only cache data that is actively requested from database

* **Write Through** 
  * Add or Update cachce when database is updated
  * each write requires 2 calls

* **Cache Evictions TTL**
  * You delete an item explicitly in cache
  * Item is removed because the memory is full and it's not recently used
  * You set up a time to leave (TTL)
  * Use cases:
    * Leaderboards
    * Comments 
    * Activity streams

## Brain Dump
* Cache Hit - if application manages to read from ElastiCache
* Cache Miss - if it cannot fetch from ElastiCache, then application should read directly to RDS and write to cache
