# AWS X-Ray

* Visual analysis of our applications
* It shows a diagram of flow of clients
* Tracing is end to end way to following a "request"
* Each component dealing with the request adds its own "trace"
* Tracing is made of segments (+ sub segments)

## How to enable X-Ray
* Use the AWS X-Ray SDK
  * The application SDK will capture:
    * calls to AWS serices
    * HTTP / HTTPS requests
    * Database Calls (RDS)
    * Queue Calls (SQS)

* Install X-Ray daemon or enable X-Ray AWS Integration
  * X-Ray daemon works as a low level UP packet interceptor
  * AWS Lambda already runs the X-Ray daemon for you
  * Each application mush have the IAM rights to write to X-Ray

## X-Ray Troubleshooting
* If X-Ray is not working on EC2
  * Ensure the EC2 IAM role has the proper permissions
  * Ensure the EC2 instance is running the X-Ray Daemon

* To enable AWS Lambda:
  * Ensure it has an IAM execution role with proper policy (AWSX-RayWriteOnlyAccess)
  * Ensure that X-Ray is imported in code

## Exam Tips
* Segments - each application / service will send them
* Trace - segments collected together to form an end-toend trace
* Sampling - decrease the amount of requests send to X-Ray to reduce cost
* Annotations - Key Value pairs used to **index** traces and use with filters
* Metadata - Key Value parise, **not indexed** not used for searching

* X-Ray on EC2 / On-Premise:
  * Linux system must run the X-Ray Daemon
  * IAM instance role if EC2, other AWS credentials on on-premise instance

* X-Ray on Lambda
  * Make sure X-Ray integration is ticked on Lambda 
  * IAM role is Lambda role

* X-Ray on Beanstalk
  * Set configuration on EB console
  * Use beanstalk extension (.ebextensions/xray-daemon.config)

* X-Ray on ECS / EKS / Fargate (Docker):
  * Create a docker image that runs the Daemon / or use the official X-Ray Docker image
  * Ensure port mappings and network settings are correct and IAM task roles are defined

## Sample Code integration
```
var app = express();

var AWSXRay = require('aws-xray-sdk');

app.use(AWSXRay.express.openSegment('MyApp'));

app.get('/', function(req, res) {
  res.sender('index');
});

app.use(AWSXRay.express.closeSegment());
```

## Sampling Rules
* You control the amount of data that you record 
* You can modity sampling rules without changing your code
* By Default X-Ray SDK records the first request each second and five percent of any additional requests
* One request per second is the **reservoir** 
* Five percent is the **rate**

## X-Ray APIs
* PutTraceSegments - Uploads segment document to X-Ray
* PutTelemetryRecords - Used by X-Ray Daemon to upload telemetry
* GetSamplingRules - Retrieve all sampling rules
* GetSamplingTargets & GetSamplingStatisticsSummaries - advanced
* GetServiceGraph - main graph
* BatchGetTraces - Retrieves a list of traces specified by ID
* GetTraceSummaries - Retrieves IDs and annotations for traces available for a specified time frame
* GetTraceGraph - retrives a service graph for one or more specific trace IDs