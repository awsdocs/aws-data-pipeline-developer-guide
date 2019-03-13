# SqlActivity<a name="dp-object-sqlactivity"></a>

Runs an SQL query \(script\) on a database\.

## Example<a name="sqlactivity-example"></a>

The following is an example of this object type\.

```
{
  "id" : "MySqlActivity",
  "type" : "SqlActivity",
  "database" : { "ref": "MyDatabaseID" },
  "script" : "SQLQuery" | "scriptUri" : s3://scriptBucket/query.sql,
  "schedule" : { "ref": "MyScheduleID" },
}
```

## Syntax<a name="sqlactivity-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| database | The database on which to run the supplied SQL script\. | Reference Object, e\.g\. "database":\{"ref":"myDatabaseId"\} | 


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule |  This object is invoked within the execution of a schedule interval\. You must specify a schedule reference to another object to set the dependency execution order for this object\. You can set a schedule explicitly on the object, for example, by specifying `"schedule": {"ref": "DefaultSchedule"}`\.  In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\.  If the pipeline has a tree of schedules nested within the master schedule, create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html)  | Reference Object, e\.g\. "schedule":\{"ref":"myScheduleId"\} | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| script | The SQL script to run\. You must specify script or scriptUri\. When the script is stored in Amazon S3, then script is not evaluated as an expression\. Specifying multiple values for scriptArgument is helpful when the script is stored in Amazon S3\. | String | 
| scriptUri | A URI specifying the location of an SQL script to execute in this activity\. | String | 


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
| input | Location of the input data\. | Reference Object, e\.g\. "input":\{"ref":"myDataNodeId"\} | 
| lateAfterTimeout | The time period since the scheduled start of the pipeline within which the object run must start\. | Period | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | Maximum number attempt retries on failure | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, e\.g\. "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed in the time period since the scheduled start of the pipeline as specified by 'lateAfterTimeout'\. | Reference Object, e\.g\. "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object, e\.g\. "onSuccess":\{"ref":"myActionId"\} | 
| output | Location of the output data\. This is only useful for referencing from within a script \(for example \#\{output\.tablename\}\) and for creating the output table by setting 'createTableSql' in the output data node\. The output of the SQL query is not written to the output data node\. | Reference Object, e\.g\. "output":\{"ref":"myDataNodeId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| precondition | Optionally define a precondition\. A data node is not marked "READY" until all preconditions have been met\. | Reference Object, e\.g\. "precondition":\{"ref":"myPreconditionId"\} | 
| queue | \[Amazon Redshift only\] Corresponds to the query\_group setting in Amazon Redshift, which allows you to assign and prioritize concurrent activities based on their placement in queues\. Amazon Redshift limits the number of simultaneous connections to 15\. For more information, see [Assigning Queries to Queues](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-executing-queries.html) in the Amazon Redshift Database Developer Guide\. | String | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| scheduleType |  Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of interval or end of the interval\. Values are: `cron`, `ondemand`, and `timeseries`\.  `timeseries` scheduling means instances are scheduled at the end of each interval\. `cron` scheduling means instances are scheduled at the beginning of each interval\.  An `ondemand` schedule allows you to run a pipeline one time per activation\. This means you do not have to clone or re\-create the pipeline to run it again\. If you use an `ondemand` schedule, it must be specified in the default object and must be the only `scheduleType` specified for objects in the pipeline\. To use `ondemand` pipelines, call the `ActivatePipeline` operation for each subsequent run\.  | Enumeration | 
| scriptArgument | A list of variables for the script\. You can alternatively put expressions directly into the script field\. Multiple values for scriptArgument are helpful when the script is stored in Amazon S3\. Example: \#\{format\(@scheduledStartTime, "YY\-MM\-DD HH:MM:SS"\}\\n\#\{format\(plusPeriod\(@scheduledStartTime, "1 day"\), "YY\-MM\-DD HH:MM:SS"\} | String | 


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
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | Id of the pipeline to which this object belongs to\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects\. | String | 