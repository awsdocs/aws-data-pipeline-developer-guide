# CopyActivity<a name="dp-object-copyactivity"></a>

Copies data from one location to another\. `CopyActivity` supports [S3DataNode](dp-object-s3datanode.md) and [SqlDataNode](dp-object-sqldatanode.md) as input and output and the copy operation is normally performed record\-by\-record\. However, `CopyActivity` provides a high\-performance Amazon S3 to Amazon S3 copy when all the following conditions are met:
+ The input and output are S3DataNodes
+ The `dataFormat` field is the same for input and output

If you provide compressed data files as input and do not indicate this using the `compression` field on the S3 data nodes, `CopyActivity` might fail\. In this case, `CopyActivity` does not properly detect the end of record character and the operation fails\. Further, `CopyActivity` supports copying from a directory to another directory and copying a file to a directory, but record\-by\-record copy occurs when copying a directory to a file\. Finally, `CopyActivity` does not support copying multipart Amazon S3 files\. 

`CopyActivity` has specific limitations to its CSV support\. When you use an S3DataNode as input for `CopyActivity`, you can only use a Unix/Linux variant of the CSV data file format for the Amazon S3 input and output fields\. The Unix/Linux variant requires the following: 
+ The separator must be the "," \(comma\) character\.
+ The records are not quoted\.
+ The default escape character is ASCII value 92 \(backslash\)\.
+ The end of record identifier is ASCII value 10 \(or "\\n"\)\.

Windows\-based systems typically use a different end\-of\-record character sequence: a carriage return and line feed together \(ASCII value 13 and ASCII value 10\)\. You must accommodate this difference using an additional mechanism, such as a pre\-copy script to modify the input data, to ensure that `CopyActivity` can properly detect the end of a record; otherwise, the `CopyActivity` fails repeatedly\.

When using `CopyActivity` to export from a PostgreSQL RDS object to a TSV data format, the default NULL character is \\n\.

## Example<a name="copyactivity-example"></a>

The following is an example of this object type\. This object references three other objects that you would define in the same pipeline definition file\. `CopyPeriod` is a `Schedule` object and `InputData` and `OutputData` are data node objects\.

```
{
  "id" : "S3ToS3Copy",
  "type" : "CopyActivity",
  "schedule" : { "ref" : "CopyPeriod" },
  "input" : { "ref" : "InputData" },
  "output" : { "ref" : "OutputData" },
  "runsOn" : { "ref" : "MyEc2Resource" }
}
```

## Syntax<a name="copyactivity-syntax"></a>


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Users must specify a schedule reference to another object to set the dependency execution order for this object\. Users can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), users can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html) | Reference Object, e\.g\. "schedule":\{"ref":"myScheduleId"\} | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| runsOn | The computational resource to run the activity or command\. For example, an Amazon EC2 instance or Amazon EMR cluster\. | Reference Object, e\.g\. "runsOn":\{"ref":"myResourceId"\} | 
| workerGroup | The worker group\. This is used for routing tasks\. If you provide a runsOn value and workerGroup exists, workerGroup is ignored\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| attemptStatus | Most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| dependsOn | Specify dependency on another runnable object\. | Reference Object, e\.g\. "dependsOn":\{"ref":"myActivityId"\} | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun | Enumeration | 
| input | The input data source\. | Reference Object, e\.g\. "input":\{"ref":"myDataNodeId"\} | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | Maximum number attempt retries on failure | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, e\.g\. "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, e\.g\. "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object, e\.g\. "onSuccess":\{"ref":"myActionId"\} | 
| output | The output data source\. | Reference Object, e\.g\. "output":\{"ref":"myDataNodeId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| precondition | Optionally define a precondition\. A data node is not marked "READY" until all preconditions have been met\. | Reference Object, e\.g\. "precondition":\{"ref":"myPreconditionId"\} | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of interval or end of the interval\. Time Series Style Scheduling means instances are scheduled at the end of each interval and Cron Style Scheduling means instances are scheduled at the beginning of each interval\. An on\-demand schedule allows you to run a pipeline one time per activation\. This means you do not have to clone or re\-create the pipeline to run it again\. If you use an on\-demand schedule it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use on\-demand pipelines, you simply call the ActivatePipeline operation for each subsequent run\. Values are: cron, ondemand, and timeseries\. | Enumeration | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, e\.g\. "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of dependency chain the object failed on\. | Reference Object, e\.g\. "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
| emrStepLog | EMR step logs available only on EMR activity attempts | String | 
| errorId | The errorId if this object failed\. | String | 
| errorMessage | The errorMessage if this object failed\. | String | 
| errorStackTrace | The error stack trace if this object failed\. | String | 
| @finishedTime | The time at which this object finished its execution\. | DateTime | 
| hadoopJobLog | Hadoop job logs available on attempts for EMR\-based activities\. | String | 
| @healthStatus | The health status of the object which reflects success or failure of the last object instance that reached a terminated state\. | String | 
| @healthStatusFromInstanceId | Id of the last instance object that reached a terminated state\. | String | 
| @healthStatusUpdatedTime | Time at which the health status was updated last time\. | DateTime | 
| hostname | The host name of client that picked up the task attempt\. | String | 
| @lastDeactivatedTime | The time at which this object was last deactivated\. | DateTime | 
| @latestCompletedRunTime | Time the latest run for which the execution completed\. | DateTime | 
| @latestRunTime | Time the latest run for which the execution was scheduled\. | DateTime | 
| @nextRunTime | Time of run to be scheduled next\. | DateTime | 
| reportProgressTime | Most recent time that remote activity reported progress\. | DateTime | 
| @scheduledEndTime | Schedule end time for object | DateTime | 
| @scheduledStartTime | Schedule start time for object | DateTime | 
| @status | The status of this object\. | String | 
| @version | Pipeline version the object was created with\. | String | 
| @waitingOn | Description of list of dependencies this object is waiting on\. | Reference Object, e\.g\. "waitingOn":\{"ref":"myRunnableObjectId"\} | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object | String | 
| @pipelineId | Id of the pipeline to which this object belongs to | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects | String | 

## See Also<a name="copyactivity-seealso"></a>
+ [ShellCommandActivity](dp-object-shellcommandactivity.md)
+ [EmrActivity](dp-object-emractivity.md)
+ [Export MySQL Data to Amazon S3 Using AWS Data Pipeline](dp-copydata-mysql.md)