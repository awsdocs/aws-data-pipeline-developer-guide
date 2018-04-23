# AWS Data Pipeline Limits<a name="dp-limits"></a>

 To ensure that there is capacity for all users, AWS Data Pipeline imposes limits on the resources that you can allocate and the rate at which you can allocate resources\.

**Topics**
+ [Account Limits](#dp-limits-account)
+ [Web Service Call Limits](#dp-limits-web-service)
+ [Scaling Considerations](#dp-scaling-considerations)

## Account Limits<a name="dp-limits-account"></a>

The following limits apply to a single AWS account\. If you require additional capacity, you can use the [Amazon Web Services Support Center request form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-datapipe) to increase your capacity\.


| Attribute | Limit | Adjustable | 
| --- | --- | --- | 
| Number of pipelines | 100 | Yes | 
| Number of objects per pipeline | 100 | Yes | 
| Number of active instances per object | 5 | Yes | 
| Number of fields per object | 50 | No | 
| Number of UTF8 bytes per field name or identifier | 256 | No | 
| Number of UTF8 bytes per field | 10,240 | No | 
| Number of UTF8 bytes per object | 15,360 \(including field names\) | No | 
| Rate of creation of an instance from an object | 1 per 5 minutes | No | 
| Retries of a pipeline activity | 5 per task | No | 
| Minimum delay between retry attempts | 2 minutes | No | 
| Minimum scheduling interval | 15 minutes | No | 
| Maximum number of roll\-ups into a single object | 32 | No | 
| Maximum number of EC2 instances per Ec2Resource object | 1 | No | 

## Web Service Call Limits<a name="dp-limits-web-service"></a>

 AWS Data Pipeline limits the rate at which you can call the web service API\. These limits also apply to AWS Data Pipeline agents that call the web service API on your behalf, such as the console, CLI, and Task Runner\. 

The following limits apply to a single AWS account\. This means the total usage on the account, including that by IAM users, cannot exceed these limits\.

 The burst rate lets you save up web service calls during periods of inactivity and expend them all in a short amount of time\. For example, CreatePipeline has a regular rate of one call each five seconds\. If you don't call the service for 30 seconds, you have six calls saved up\. You could then call the web service six times in a second\. Because this is below the burst limit and keeps your average calls at the regular rate limit, your calls are not throttled\. 

 If you exceed the rate limit and the burst limit, your web service call fails and returns a throttling exception\. The default implementation of a worker, Task Runner, automatically retries API calls that fail with a throttling exception\. Task Runner has a back off so that subsequent attempts to call the API occur at increasingly longer intervals\. If you write a worker, we recommend that you implement similar retry logic\. 

These limits are applied against an individual AWS account\. 


| API  | Regular rate limit | Burst limit | 
| --- | --- | --- | 
| ActivatePipeline | 1 call per second | 100 calls | 
| CreatePipeline | 1 call per second | 100 calls | 
| DeletePipeline | 1 call per second | 100 calls | 
| DescribeObjects | 2 calls per second | 100 calls | 
| DescribePipelines | 1 call per second | 100 calls | 
| GetPipelineDefinition | 1 call per second | 100 calls | 
| PollForTask | 2 calls per second | 100 calls | 
| ListPipelines | 1 call per second | 100 calls | 
| PutPipelineDefinition | 1 call per second | 100 calls | 
| QueryObjects | 2 calls per second | 100 calls | 
| ReportTaskProgress | 10 calls per second | 100 calls | 
| SetTaskStatus | 10 calls per second | 100 calls | 
| SetStatus | 1 call per second | 100 calls | 
| ReportTaskRunnerHeartbeat | 1 call per second | 100 calls | 
| ValidatePipelineDefinition | 1 call per second | 100 calls | 

## Scaling Considerations<a name="dp-scaling-considerations"></a>

 AWS Data Pipeline scales to accommodate a huge number of concurrent tasks and you can configure it to automatically create the resources necessary to handle large workloads\. These automatically created resources are under your control and count against your AWS account resource limits\. For example, if you configure AWS Data Pipeline to automatically create a 20\-node Amazon EMR cluster to process data and your AWS account has an EC2 instance limit set to 20, you may inadvertently exhaust your available backfill resources\. As a result, consider these resource restrictions in your design or increase your account limits accordingly\. 

If you require additional capacity, you can use the [Amazon Web Services Support Center request form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-datapipe) to increase your capacity\.