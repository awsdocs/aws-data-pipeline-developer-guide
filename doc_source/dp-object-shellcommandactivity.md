# ShellCommandActivity<a name="dp-object-shellcommandactivity"></a>

 Runs a command or script\. You can use `ShellCommandActivity` to run time\-series or cron\-like scheduled tasks\. 

When the `stage` field is set to true and used with an `S3DataNode`, `ShellCommandActivity` supports the concept of staging data, which means that you can move data from Amazon S3 to a stage location, such as Amazon EC2 or your local environment, perform work on the data using scripts and the `ShellCommandActivity`, and move it back to Amazon S3\. In this case, when your shell command is connected to an input S3DataNode, your shell scripts operate directly on the data using `${INPUT1_STAGING_DIR}`, `${INPUT2_STAGING_DIR}`, etc\. referring to the `ShellCommandActivity` input fields\. Similarly, output from the shell\-command can be staged in an output directory to be automatically pushed to Amazon S3, referred to by `${OUTPUT1_STAGING_DIR}`, `${OUTPUT2_STAGING_DIR}`, and so on\. These expressions can pass as command\-line arguments to the shell\-command for you to use in data transformation logic\.

`ShellCommandActivity` returns Linux\-style error codes and strings\. If a `ShellCommandActivity` results in error, the `error` returned will be a non\-zero value\.

## Example<a name="shellcommandactivity-example"></a>

The following is an example of this object type\.

```
{
  "id" : "CreateDirectory",
  "type" : "ShellCommandActivity",
  "command" : "mkdir new-directory"
}
```

## Syntax<a name="shellcommandactivity-syntax"></a>


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Users must specify a schedule reference to another object to set the dependency execution order for this object\. Users can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), users can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html) | Reference Object, e\.g\. "schedule":\{"ref":"myScheduleId"\} | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| command | The command to run\. Use $ to reference positional parameters and scriptArgument to specify the parameters for the command\. This value and any associated parameters must function in the environment from which you are running the Task Runner\. | String | 
| scriptUri | An Amazon S3 URI path for a file to download and run as a shell command\. Only one scriptUri or command field should be present\. scriptUri cannot use parameters, use command instead\. | String | 


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
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | Maximum number attempt retries on failure | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, e\.g\. "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, e\.g\. "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object, e\.g\. "onSuccess":\{"ref":"myActionId"\} | 
| output | Location of the output data\. | Reference Object, e\.g\. "output":\{"ref":"myDataNodeId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| precondition | Optionally define a precondition\. A data node is not marked "READY" until all preconditions have been met\. | Reference Object, e\.g\. "precondition":\{"ref":"myPreconditionId"\} | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set then remote activities that do not report progres for the specified period may be considered stalled and so retried\. | Period | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of interval or end of the interval\. Time Series Style Scheduling means instances are scheduled at the end of each interval and Cron Style Scheduling means instances are scheduled at the beginning of each interval\. An on\-demand schedule allows you to run a pipeline one time per activation\. This means you do not have to clone or re\-create the pipeline to run it again\. If you use an on\-demand schedule it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use on\-demand pipelines, you simply call the ActivatePipeline operation for each subsequent run\. Values are: cron, ondemand, and timeseries\. | Enumeration | 
| scriptArgument | A JSON\-formatted array of strings to pass to the command specified by command\. For example, if command is echo $1 $2, you can specify scriptArgument as "param1", "param2"\. If you had multiple arguments and parameters, you can pass scriptArgument as follows: "scriptArgument":"arg1","scriptArgument":"param1","scriptArgument":"arg2","scriptArgument":"param2" \. scriptArgument can only be used with command, and using with scriptUri causes an error\. | String | 
| stage | Determines whether staging is enabled and allows your shell commands to have access to the staged\-data variables, such as $\{INPUT1\_STAGING\_DIR\} and $\{OUTPUT1\_STAGING\_DIR\}\. | Boolean | 
| stderr | The path that receives redirected system error messages from the command\. If you use the runsOn field, this must be an Amazon S3 path because of the transitory nature of the resource running your activity\. However if you specify the workerGroup field, a local file path is permitted\. | String | 
| stdout | The Amazon S3 path that receives redirected output from the command\. If you use the runsOn field, this must be an Amazon S3 path because of the transitory nature of the resource running your activity\. However if you specify the workerGroup field, a local file path is permitted\. | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, e\.g\. "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of depedency chain the object failed on\. | Reference Object, e\.g\. "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
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

## See Also<a name="shellcommandactivity-seealso"></a>

+ [CopyActivity](dp-object-copyactivity.md)

+ [EmrActivity](dp-object-emractivity.md)