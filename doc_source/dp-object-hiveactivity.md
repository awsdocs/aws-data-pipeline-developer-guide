# HiveActivity<a name="dp-object-hiveactivity"></a>

Runs a Hive query on an EMR cluster\. `HiveActivity` makes it easier to set up an Amazon EMR activity and automatically creates Hive tables based on input data coming in from either Amazon S3 or Amazon RDS\. All you need to specify is the HiveQL to run on the source data\. AWS Data Pipeline automatically creates Hive tables with `${input1}`, `${input2}`, and so on, based on the input fields in the `HiveActivity` object\. 

For Amazon S3 inputs, the `dataFormat` field is used to create the Hive column names\. 

For MySQL \(Amazon RDS\) inputs, the column names for the SQL query are used to create the Hive column names\.

**Note**  
This activity uses the Hive [CSV Serde](https://cwiki.apache.org/confluence/display/Hive/CSV+Serde)\.

## Example<a name="hiveactivity-example"></a>

The following is an example of this object type\. This object references three other objects that you define in the same pipeline definition file\. `MySchedule` is a `Schedule` object and `MyS3Input` and `MyS3Output` are data node objects\.

```
{
  "name" : "ProcessLogData",
  "id" : "MyHiveActivity",
  "type" : "HiveActivity",
  "schedule" : { "ref": "MySchedule" },
  "hiveScript" : "INSERT OVERWRITE TABLE ${output1} select host,user,time,request,status,size from ${input1};",
  "input" : { "ref": "MyS3Input" },
  "output" : { "ref": "MyS3Output" },
  "runsOn" : { "ref": "MyEmrCluster" }
}
```

## Syntax<a name="hiveactivity-syntax"></a>


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Specify a schedule reference to another object to set the dependency execution order for this object\. You can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), you can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html)\. | Reference Object, e\.g\. "schedule":\{"ref":"myScheduleId"\} | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| hiveScript | The Hive script to run\. | String | 
| scriptUri | The location of the Hive script to run \(for example, s3://scriptLocation\)\. | String | 


****  

| Required Group | Description | Slot Type | 
| --- | --- | --- | 
| runsOn | The EMR cluster on which this HiveActivity runs\. | Reference Object, e\.g\. "runsOn":\{"ref":"myEmrClusterId"\} | 
| workerGroup | The worker group\. This is used for routing tasks\. If you provide a runsOn value and workerGroup exists, workerGroup is ignored\. | String | 
| input | The input data source\. | Reference Object, such as "input":\{"ref":"myDataNodeId"\} | 
| output | The output data source\. | Reference Object, such as "output":\{"ref":"myDataNodeId"\} | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| attemptStatus | Most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set, then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| dependsOn | Specify dependency on another runnable object\. | Reference Object, such as "dependsOn":\{"ref":"myActivityId"\} | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun\. | Enumeration | 
| hadoopQueue | The Hadoop scheduler queue name on which the job will be submitted\. | String | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | The maximum number of attempt retries on failure\. | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, such as "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, such as "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object, such as "onSuccess":\{"ref":"myActionId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, such as "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| postActivityTaskConfig | Post\-activity configuration script to be run\. This consists of a URI of the shell script in Amazon S3 and a list of arguments\. | Reference Object, such as "postActivityTaskConfig":\{"ref":"myShellScriptConfigId"\} | 
| preActivityTaskConfig | Pre\-activity configuration script to be run\. This consists of a URI of the shell script in Amazon S3 and a list of arguments\. | Reference Object, such as "preActivityTaskConfig":\{"ref":"myShellScriptConfigId"\} | 
| precondition | Optionally define a precondition\. A data node is not marked "READY" until all preconditions have been met\. | Reference Object, such as "precondition":\{"ref":"myPreconditionId"\} | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| resizeClusterBeforeRunning | Resize the cluster before performing this activity to accommodate DynamoDB data nodes specified as inputs or outputs\.  If your activity uses a `DynamoDBDataNode` as either an input or output data node, and if you set the `resizeClusterBeforeRunning` to `TRUE`, AWS Data Pipeline starts using `m3.xlarge` instance types\. This overwrites your instance type choices with `m3.xlarge`, which could increase your monthly costs\.  | Boolean | 
| resizeClusterMaxInstances | A limit on the maximum number of instances that can be requested by the resize algorithm\. | Integer | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of interval or end of the interval\. Time Series Style Scheduling means instances are scheduled at the end of each interval and Cron Style Scheduling means instances are scheduled at the beginning of each interval\. An on\-demand schedule allows you to run a pipeline one time per activation\. This means you do not have to clone or re\-create the pipeline to run it again\. If you use an on\-demand schedule it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use on\-demand pipelines, you simply call the ActivatePipeline operation for each subsequent run\. Values are: cron, ondemand, and timeseries\. | Enumeration | 
| scriptVariable | Specifies script variables for Amazon EMR to pass to Hive while running a script\. For example, the following example script variables would pass a SAMPLE and FILTER\_DATE variable to Hive: SAMPLE=s3://elasticmapreduce/samples/hive\-ads and FILTER\_DATE=\#\{format\(@scheduledStartTime,'YYYY\-MM\-dd'\)\}%\. This field accepts multiple values and works with both script and scriptUri fields\. In addition, scriptVariable functions regardless of whether stage is set to true or false\. This field is especially useful to send dynamic values to Hive using AWS Data Pipeline expressions and functions\. | String | 
| stage | Determines whether staging is enabled before or after running the script\. Not permitted with Hive 11, so use an Amazon EMR AMI version 3\.2\.0 or greater\. | Boolean | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, such as "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain the object failed on\. | Reference Object, such as "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
| emrStepLog | Amazon EMR step logs available only on EMR activity attempts\. | String | 
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
| @scheduledEndTime | Schedule end time for an object\. | DateTime | 
| @scheduledStartTime | Schedule start time for an object\. | DateTime | 
| @status | The status of this object\. | String | 
| @version | Pipeline version the object was created with\. | String | 
| @waitingOn | Description of list of dependencies this object is waiting on\. | Reference Object, such as "waitingOn":\{"ref":"myRunnableObjectId"\} | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | Id of the pipeline to which this object belongs\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects\. | String | 

## See Also<a name="hiveactivity-seealso"></a>
+ [ShellCommandActivity](dp-object-shellcommandactivity.md)
+ [EmrActivity](dp-object-emractivity.md)