# S3DataNode<a name="dp-object-s3datanode"></a>

 Defines a data node using Amazon S3\. By default, the S3DataNode uses server\-side encryption\. If you would like to disable this, set s3EncryptionType to NONE\. 

**Note**  
When you use an `S3DataNode` as input to `CopyActivity`, only the CSV and TSV data formats are supported\.

## Example<a name="s3datanode-example"></a>

The following is an example of this object type\. This object references another object that you'd define in the same pipeline definition file\. `CopyPeriod` is a `Schedule` object\.

```
{
  "id" : "OutputData",
  "type" : "S3DataNode",
  "schedule" : { "ref" : "CopyPeriod" },
  "filePath" : "s3://myBucket/#{@scheduledStartTime}.csv"
}
```

## Syntax<a name="s3datanode-syntax"></a>


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Users must specify a schedule reference to another object to set the dependency execution order for this object\. Users can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), users can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html) | Reference Object, e\.g\. "schedule":\{"ref":"myScheduleId"\} | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| attemptStatus | Most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| compression | The type of compression for the data described by the S3DataNode\. "none" is no compression and "gzip" is compressed with the gzip algorithm\. This field is only supported for use with Amazon Redshift and when you use S3DataNode with CopyActivity\. | Enumeration | 
| dataFormat | DataFormat for the data described by this S3DataNode\. | Reference Object, e\.g\. "dataFormat":\{"ref":"myDataFormatId"\} | 
| dependsOn | Specify dependency on another runnable object | Reference Object, e\.g\. "dependsOn":\{"ref":"myActivityId"\} | 
| directoryPath | Amazon S3 directory path as a URI: s3://my\-bucket/my\-key\-for\-directory\. You must provide either a filePath or directoryPath value\. | String | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun | Enumeration | 
| filePath | The path to the object in Amazon S3 as a URI, for example: s3://my\-bucket/my\-key\-for\-file\. You must provide either a filePath or directoryPath value\. These represent a folder and a file name\. Use the directoryPath value to accommodate multiple files in a directory\. | String | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| manifestFilePath | The Amazon S3 path to a manifest file in the format supported by Amazon Redshift\. AWS Data Pipeline uses the manifest file to copy the specified Amazon S3 files into the table\. This field is valid only when a RedShiftCopyActivity references the S3DataNode\. | String | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | Maximum number attempt retries on failure | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, e\.g\. "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, e\.g\. "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object, e\.g\. "onSuccess":\{"ref":"myActionId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| precondition | Optionally define a precondition\. A data node is not marked "READY" until all preconditions have been met\. | Reference Object, e\.g\. "precondition":\{"ref":"myPreconditionId"\} | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| runsOn | The computational resource to run the activity or command\. For example, an Amazon EC2 instance or Amazon EMR cluster\. | Reference Object, e\.g\. "runsOn":\{"ref":"myResourceId"\} | 
| s3EncryptionType | Overrides the Amazon S3 encryption type\. Values are SERVER\_SIDE\_ENCRYPTION or NONE\. Server\-side encryption is enabled by default\.  | Enumeration | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of interval or end of the interval\. Time Series Style Scheduling means instances are scheduled at the end of each interval and Cron Style Scheduling means instances are scheduled at the beginning of each interval\. An on\-demand schedule allows you to run a pipeline one time per activation\. This means you do not have to clone or re\-create the pipeline to run it again\. If you use an on\-demand schedule it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use on\-demand pipelines, you simply call the ActivatePipeline operation for each subsequent run\. Values are: cron, ondemand, and timeseries\. | Enumeration | 
| workerGroup | The worker group\. This is used for routing tasks\. If you provide a runsOn value and workerGroup exists, workerGroup is ignored\. | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, e\.g\. "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain the object failed on\. | Reference Object, e\.g\. "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
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

## See Also<a name="s3datanode-seealso"></a>
+ [MySqlDataNode](dp-object-mysqldatanode.md)