# AppSync
* is a managed service that uses GraphQL
* GraphQL - Makes it easy for applications to get exactly the data they need
* This includes combining data from one or more sources:
  * NoSQL data stores, RDS, HTTP, APIs...
  * integrates with DynamoDB, Aurora, ElasticSearch
  * Custom sources with AWS Lambda

* Retreive data in real-time with WebSocker or MQTT on WebSocket
* For mobile apps:  local data access & data synchronization

## Sample Question:
Which of the following does NOT allow for a real-time WebSocket API?
**Ans:** DynamoDB - does not push changes to the users and does not have a two-way communication. It's just a request/response database

## GraphQL Sent by Clients
```
{
  human(id: 1002) {
    name
    appearsIn
    starships {
      name
    }
  }
}
```

## GraphQL Response in JSON
```
{
  "data": {
    "human": {
      name: "Han Solo",
      "appearsIn": [
        "Star Wars"
      ],
      "starships": [
        {
          "name": "Millenium Falcon"
        }
      ]
    }
  }
}

```