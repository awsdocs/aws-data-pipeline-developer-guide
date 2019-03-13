# Resolving Common Problems<a name="dp-check-when-run-fails"></a>

This topic provides various symptoms of AWS Data Pipeline problems and the recommended steps to solve them\. 

**Topics**
+ [Pipeline Stuck in Pending Status](#dp-pipeline-doesnt-start)
+ [Pipeline Component Stuck in Waiting for Runner Status](#dp-waiting-for-runner)
+ [Pipeline Component Stuck in WAITING\_ON\_DEPENDENCIES Status](#dp-runs-stay-pending)
+ [Run Doesn't Start When Scheduled](#dp-run-doesnt-start-scheduled)
+ [Pipeline Components Run in Wrong Order](#dp-out-of-order)
+ [EMR Cluster Fails With Error: The security token included in the request is invalid](#dp-securitytoken)
+ [Insufficient Permissions to Access Resources](#dp-insufficient-permissions)
+ [Status Code: 400 Error Code: PipelineNotFoundException](#dp-error-code-400)
+ [Creating a Pipeline Causes a Security Token Error](#dp-bad-token)
+ [Cannot See Pipeline Details in the Console](#dp-no-details-shown)
+ [Error in remote runner Status Code: 404, AWS Service: Amazon S3](#dp-error-code-s3-404)
+ [Access Denied \- Not Authorized to Perform Function datapipeline:](#dp-access-denied)
+ [Older Amazon EMR AMIs May Create False Data for Large CSV Files](#AMIs-false-data-large-CSV)
+ [Increasing AWS Data Pipeline Limits](#dp-increase-limits)

## Pipeline Stuck in Pending Status<a name="dp-pipeline-doesnt-start"></a>

A pipeline that appears stuck in the PENDING status indicates that a pipeline has not yet been activated, or activation failed due to an error in the pipeline definition\. Ensure that you did not receive any errors when you submitted your pipeline using the AWS Data Pipeline CLI or when you attempted to save or activate your pipeline using the AWS Data Pipeline console\. Additionally, check that your pipeline has a valid definition\.

To view your pipeline definition on the screen using the CLI:

```
datapipeline --get --id df-EXAMPLE_PIPELINE_ID
```

Ensure that the pipeline definition is complete, check your closing braces, verify required commas, check for missing references, and other syntax errors\. It is best to use a text editor that can visually validate the syntax of JSON files\.

## Pipeline Component Stuck in Waiting for Runner Status<a name="dp-waiting-for-runner"></a>

If your pipeline is in the SCHEDULED state and one or more tasks appear stuck in the WAITING\_FOR\_RUNNER state, ensure that you set a valid value for either the runsOn or workerGroup fields for those tasks\. If both values are empty or missing, the task cannot start because there is no association between the task and a worker to perform the tasks\. In this situation, you've defined work but haven't defined what computer does the work\. If applicable, verify that the workerGroup value assigned to the pipeline component is exactly the same name and case as the workerGroup value that you configured for Task Runner\. 

**Note**  
If you provide a runsOn value and workerGroup exists, workerGroup is ignored\.

Another potential cause of this problem is that the endpoint and access key provided to Task Runner is not the same as the AWS Data Pipeline console or the computer where the AWS Data Pipeline CLI tools are installed\. You might have created new pipelines with no visible errors, but Task Runner polls the wrong location due to the difference in credentials, or polls the correct location with insufficient permissions to identify and run the work specified by the pipeline definition\. 

## Pipeline Component Stuck in WAITING\_ON\_DEPENDENCIES Status<a name="dp-runs-stay-pending"></a>

If your pipeline is in the `SCHEDULED` state and one or more tasks appear stuck in the `WAITING_ON_DEPENDENCIES` state, make sure your pipeline's initial preconditions have been met\. If the preconditions of the first object in the logic chain are not met, none of the objects that depend on that first object can move out of the `WAITING_ON_DEPENDENCIES` state\. 

For example, consider the following excerpt from a pipeline definition\. In this case, the `InputData` object has a precondition 'Ready' specifying that the data must exist before the InputData object is complete\. If the data does not exist, the InputData object remains in the `WAITING_ON_DEPENDENCIES` state, waiting for the data specified by the path field to become available\. Any objects that depend on InputData likewise remain in a `WAITING_ON_DEPENDENCIES` state waiting for the InputData object to reach the `FINISHED` state\. 

```
{
    "id": "InputData",
    "type": "S3DataNode",
    "filePath": "s3://elasticmapreduce/samples/wordcount/wordSplitter.py",
    "schedule":{"ref":"MySchedule"},
    "precondition": "Ready"      
},
{
    "id": "Ready",
    "type": "Exists"
...
```

 Also, check that your objects have the proper permissions to access the data\. In the preceding example, if the information in the credentials field did not have permissions to access the data specified in the path field, the InputData object would get stuck in a `WAITING_ON_DEPENDENCIES` state because it cannot access the data specified by the path field, even if that data exists\. 

It is also possible that a resource communicating with Amazon S3 does not have a public IP address associated with it\. For example, an `Ec2Resource` in a public subnet must have a public IP address associated with it\. 

Lastly, under certain conditions, resource instances can reach the `WAITING_ON_DEPENDENCIES` state much earlier than their associated activities are scheduled to start, which may give the impression that the resource or the activity is failing\. For more information about the behavior of resources and the schedule type setting, see the *Resources Ignore Schedule Type* section in the [Scheduling Pipelines](dp-concepts-schedules.md) topic\.

## Run Doesn't Start When Scheduled<a name="dp-run-doesnt-start-scheduled"></a>

Check that you chose the correct schedule type that determines whether your task starts at the beginning of the schedule interval \(Cron Style Schedule Type\) or at the end of the schedule interval \(Time Series Schedule Type\)\.

Additionally, check that you have properly specified the dates in your schedule objects and that the startDateTime and endDateTime values are in UTC format, such as in the following example:

```
{
    "id": "MySchedule",
    "startDateTime": "2012-11-12T19:30:00",
    "endDateTime":"2012-11-12T20:30:00",
    "period": "1 Hour",
    "type": "Schedule"
},
```

## Pipeline Components Run in Wrong Order<a name="dp-out-of-order"></a>

You might notice that the start and end times for your pipeline components are running in the wrong order, or in a different sequence than you expect\. It is important to understand that pipeline components can start running simultaneously if their preconditions are met at start\-up time\. In other words, pipeline components do not execute sequentially by default; if you need a specific execution order, you must control the execution order with preconditions and `dependsOn` fields\. 

Verify that you are using the `dependsOn` field populated with a reference to the correct prerequisite pipeline components, and that all the necessary pointers between components are present to achieve the order you require\. 

## EMR Cluster Fails With Error: The security token included in the request is invalid<a name="dp-securitytoken"></a>

Verify your IAM roles, policies, and trust relationships as described in [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)\.

## Insufficient Permissions to Access Resources<a name="dp-insufficient-permissions"></a>

Permissions that you set on IAM roles determine whether AWS Data Pipeline can access your EMR clusters and EC2 instances to run your pipelines\. Additionally, IAM provides the concept of trust relationships that go further to allow the creation of resources on your behalf\. For example, when you create a pipeline that uses an EC2 instance to run a command to move data, AWS Data Pipeline can provision this EC2 instance for you\. If you encounter problems, especially those involving resources that you can access manually but AWS Data Pipeline cannot, verify your IAM roles, policies, and trust relationships as described in [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)\.

## Status Code: 400 Error Code: PipelineNotFoundException<a name="dp-error-code-400"></a>

This error means that your IAM default roles might not have the required permissions necessary for AWS Data Pipeline to function correctly\. For more information, see [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)\. 

## Creating a Pipeline Causes a Security Token Error<a name="dp-bad-token"></a>

You receive the following error when you try to create a pipeline:

Failed to create pipeline with 'pipeline\_name'\. Error: UnrecognizedClientException \- The security token included in the request is invalid\.

## Cannot See Pipeline Details in the Console<a name="dp-no-details-shown"></a>

The AWS Data Pipeline console pipeline filter applies to the *scheduled* start date for a pipeline, without regard to when the pipeline was submitted\. It is possible to submit a new pipeline using a scheduled start date that occurs in the past, which the default date filter may not show\. To see the pipeline details, change your date filter to ensure that the scheduled pipeline start date fits within the date range filter\. 

## Error in remote runner Status Code: 404, AWS Service: Amazon S3<a name="dp-error-code-s3-404"></a>

 This error means that Task Runner could not access your files in Amazon S3\. Verify that: 
+  You have credentials correctly set 
+  The Amazon S3 bucket that you are trying to access exists 
+  You are authorized to access the Amazon S3 bucket 

## Access Denied \- Not Authorized to Perform Function datapipeline:<a name="dp-access-denied"></a>

In the Task Runner logs, you may see an error that is similar to the following: 
+  ERROR Status Code: 403 
+  AWS Service: DataPipeline 
+  AWS Error Code: AccessDenied 
+ AWS Error Message: User: arn:aws:sts::XXXXXXXXXXXX:federated\-user/i\-XXXXXXXX is not authorized to perform: datapipeline:PollForTask\.

**Note**  
In this error message, PollForTask may be replaced with names of other AWS Data Pipeline permissions\.

This error message indicates that the IAM role you specified needs additional permissions necessary to interact with AWS Data Pipeline\. Ensure that your IAM role policy contains the following lines, where PollForTask is replaced with the name of the permission you want to add \(use \* to grant all permissions\)\. For more information about how to create a new IAM role and apply a policy to it, see [Managing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) in the *Using IAM* guide\. 

```
{
"Action": [ "datapipeline:PollForTask" ],
"Effect": "Allow",
"Resource": ["*"]
}
```

## Older Amazon EMR AMIs May Create False Data for Large CSV Files<a name="AMIs-false-data-large-CSV"></a>

On Amazon EMR AMIs previous to 3\.9 \(3\.8 and below\) AWS Data Pipeline uses a custom InputFormat to read and write CSV files for use with MapReduce jobs\. This is used when the service stages tables to and from Amazon S3\. An issue with this InputFormat was discovered where reading records from large CSV files may result in producing tables that are not correctly copied\. This issue was fixed in later Amazon EMR releases\. Please use Amazon EMR AMI 3\.9 or an Amazon EMR release 4\.0\.0 or greater\.

## Increasing AWS Data Pipeline Limits<a name="dp-increase-limits"></a>

Occasionally, you may exceed specific AWS Data Pipeline system limits\. For example, the default pipeline limit is 20 pipelines with 50 objects in each\. If you discover that you need more pipelines than the limit, consider merging multiple pipelines to create fewer pipelines with more objects in each\. For more information about the AWS Data Pipeline limits, see [AWS Data Pipeline Limits](dp-limits.md)\. However, if you are unable to work around the limits using the pipeline merge technique, request an increase in your capacity using this form: [Data Pipeline Limit Increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-datapipe)\.