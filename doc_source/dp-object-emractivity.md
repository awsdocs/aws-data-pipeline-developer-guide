# EmrActivity<a name="dp-object-emractivity"></a>

 Runs an EMR cluster\. 

AWS Data Pipeline uses a different format for steps than Amazon EMR; for example, AWS Data Pipeline uses comma\-separated arguments after the JAR name in the `EmrActivity` step field\. The following example shows a step formatted for Amazon EMR, followed by its AWS Data Pipeline equivalent:

```
s3://example-bucket/MyWork.jar arg1 arg2 arg3
```

```
"s3://example-bucket/MyWork.jar,arg1,arg2,arg3"
```

## Examples<a name="emractivity-example"></a>

The following is an example of this object type\. This example uses older versions of Amazon EMR\. Verify this example for correctness with the version of Amazon EMR cluster that you are using\. 

This object references three other objects that you would define in the same pipeline definition file\. `MyEmrCluster` is an `EmrCluster` object and `MyS3Input` and `MyS3Output` are `S3DataNode` objects\. 

**Note**  
In this example, you can replace the `step` field with your desired cluster string, which could be a Pig script, Hadoop streaming cluster, your own custom JAR including its parameters, or so on\.

Hadoop 2\.x \(AMI 3\.x\)

```
{
  "id" : "MyEmrActivity",
  "type" : "EmrActivity",
  "runsOn" : { "ref" : "MyEmrCluster" },
  "preStepCommand" : "scp remoteFiles localFiles",
  "step" : ["s3://mybucket/myPath/myStep.jar,firstArg,secondArg,-files,s3://mybucket/myPath/myFile.py,-input,s3://myinputbucket/path,-output,s3://myoutputbucket/path,-mapper,myFile.py,-reducer,reducerName","s3://mybucket/myPath/myotherStep.jar,..."],
  "postStepCommand" : "scp localFiles remoteFiles",
  "input" : { "ref" : "MyS3Input" },
  "output" : { "ref" : "MyS3Output" }
}
```

**Note**  
To pass arguments to an application in a step, you need to specify the Region in the path of the script, as in the following example\. In addition, you may need to escape the arguments that you pass\. For example, if you use `script-runner.jar` to run a shell script and want to pass arguments to the script, you must escape the commas that separate them\. The following step slot illustrates how to do this:   

```
"step" : "s3://eu-west-1.elasticmapreduce/libs/script-runner/script-runner.jar,s3://datapipeline/echo.sh,a\\\\,b\\\\,c"
```
This step uses `script-runner.jar` to run the `echo.sh` shell script and passes `a`, `b`, and `c` as a single argument to the script\. The first escape character is removed from the resultant argument so you may need to escape again\. For example, if you had `File\.gz` as an argument in JSON, you could escape it using `File\\\\.gz`\. However, because the first escape is discarded, you must use `File\\\\\\\\.gz `\.

## Syntax<a name="emractivity-syntax"></a>


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Specify a schedule reference to another object to set the dependency execution order for this object\. You can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), you can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html) | Reference Object, for example, "schedule":\{"ref":"myScheduleId"\} | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| runsOn | The Amazon EMR cluster on which this job will run\. | Reference Object, for example, "runsOn":\{"ref":"myEmrClusterId"\} | 
| workerGroup | The worker group\. This is used for routing tasks\. If you provide a runsOn value and workerGroup exists, workerGroup is ignored\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| attemptStatus | Most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set, then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| dependsOn | Specify dependency on another runnable object\. | Reference Object, for example, "dependsOn":\{"ref":"myActivityId"\} | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun\. | Enumeration | 
| input | The location of the input data\. | Reference Object, for example, "input":\{"ref":"myDataNodeId"\} | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | The maximum number of attempt retries on failure\. | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, for example, "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, for example, "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when the current object succeeds\. | Reference Object, for example, "onSuccess":\{"ref":"myActionId"\} | 
| output | The location of the output data\. | Reference Object, for example, "output":\{"ref":"myDataNodeId"\} | 
| parent | The parent of the current object from which slots will be inherited\. | Reference Object, for example, "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The Amazon S3 URI, such as 's3://BucketName/Prefix/' for uploading logs for the pipeline\. | String | 
| postStepCommand | Shell scripts to be run after all steps are finished\. To specify multiple scripts, up to 255, add multiple postStepCommand fields\. | String | 
| precondition | Optionally define a precondition\. A data node is not marked "READY" until all preconditions have been met\. | Reference Object, for example, "precondition":\{"ref":"myPreconditionId"\} | 
| preStepCommand | Shell scripts to be run before any steps are run\. To specify multiple scripts, up to 255, add multiple preStepCommand fields\. | String | 
| reportProgressTimeout | The timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| resizeClusterBeforeRunning |  Resize the cluster before performing this activity to accommodate DynamoDB tables specified as inputs or outputs\.   If your `EmrActivity` uses a `DynamoDBDataNode` as either an input or output data node, and if you set the `resizeClusterBeforeRunning` to `TRUE`, AWS Data Pipeline starts using `m3.xlarge` instance types\. This overwrites your instance type choices with `m3.xlarge`, which could increase your monthly costs\.   | Boolean | 
| resizeClusterMaxInstances | A limit on the maximum number of instances that can be requested by the resize algorithm\. | Integer | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of the interval, or end of the interval\. Values are: cron, ondemand, and timeseries\. The timeseries scheduling means that instances are scheduled at the end of each interval\. The cron scheduling means that instances are scheduled at the beginning of each interval\. An ondemand schedule allows you to run a pipeline one time per activation\. You do not have to clone or re\-create the pipeline to run it again\. If you use an ondemand schedule, it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use ondemand pipelines, call the ActivatePipeline operation for each subsequent run\.  | Enumeration | 
| step | One or more steps for the cluster to run\. To specify multiple steps, up to 255, add multiple step fields\. Use comma\-separated arguments after the JAR name; for example, "s3://example\-bucket/MyWork\.jar,arg1,arg2,arg3"\. | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, e\.g\. "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain the object failed on\. | Reference Object, for example, "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
| emrStepLog | Amazon EMR step logs available only on EMR activity attempts | String | 
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
| @scheduledEndTime | Schedule end time for the object\. | DateTime | 
| @scheduledStartTime | Schedule start time for the object\. | DateTime | 
| @status | The status of this object\. | String | 
| @version | Pipeline version that the object was created with\. | String | 
| @waitingOn | Description of list of dependencies this object is waiting on\. | Reference Object, for example, "waitingOn":\{"ref":"myRunnableObjectId"\} | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | ID of the pipeline to which this object belongs\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects\. | String | 

## See Also<a name="emractivity-seealso"></a>
+ [ShellCommandActivity](dp-object-shellcommandactivity.md)
+ [CopyActivity](dp-object-copyactivity.md)
+ [EmrCluster](dp-object-emrcluster.md)