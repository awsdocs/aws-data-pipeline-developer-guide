# PigActivity<a name="dp-object-pigactivity"></a>

PigActivity provides native support for Pig scripts in AWS Data Pipeline without the requirement to use `ShellCommandActivity` or `EmrActivity`\. In addition, PigActivity supports data staging\. When the stage field is set to true, AWS Data Pipeline stages the input data as a schema in Pig without additional code from the user\. 

## Example<a name="pigactivity-example"></a>

The following example pipeline shows how to use `PigActivity`\. The example pipeline performs the following steps:
+ MyPigActivity1 loads data from Amazon S3 and runs a Pig script that selects a few columns of data and uploads it to Amazon S3\.
+ MyPigActivity2 loads the first output, selects a few columns and three rows of data, and uploads it to Amazon S3 as a second output\.
+ MyPigActivity3 loads the second output data, inserts two rows of data and only the column named "fifth" to Amazon RDS\.
+ MyPigActivity4 loads Amazon RDS data, selects the first row of data, and uploads it to Amazon S3\.

```
{
  "objects": [
    {
      "id": "MyInputData1",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "directoryPath": "s3://example-bucket/pigTestInput",
      "name": "MyInputData1",
      "dataFormat": {
        "ref": "MyInputDataType1"
      },
      "type": "S3DataNode"
    },
    {
      "id": "MyPigActivity4",
      "scheduleType": "CRON",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "input": {
        "ref": "MyOutputData3"
      },
      "pipelineLogUri": "s3://example-bucket/path/",
      "name": "MyPigActivity4",
      "runsOn": {
        "ref": "MyEmrResource"
      },
      "type": "PigActivity",
      "dependsOn": {
        "ref": "MyPigActivity3"
      },
      "output": {
        "ref": "MyOutputData4"
      },
      "script": "B = LIMIT ${input1} 1; ${output1} = FOREACH B GENERATE one;",
      "stage": "true"
    },
    {
      "id": "MyPigActivity3",
      "scheduleType": "CRON",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "input": {
        "ref": "MyOutputData2"
      },
      "pipelineLogUri": "s3://example-bucket/path",
      "name": "MyPigActivity3",
      "runsOn": {
        "ref": "MyEmrResource"
      },
      "script": "B = LIMIT ${input1} 2; ${output1} = FOREACH B GENERATE Fifth;",
      "type": "PigActivity",
      "dependsOn": {
        "ref": "MyPigActivity2"
      },
      "output": {
        "ref": "MyOutputData3"
      },
      "stage": "true"
    },
    {
      "id": "MyOutputData2",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "name": "MyOutputData2",
      "directoryPath": "s3://example-bucket/PigActivityOutput2",
      "dataFormat": {
        "ref": "MyOutputDataType2"
      },
      "type": "S3DataNode"
    },
    {
      "id": "MyOutputData1",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "name": "MyOutputData1",
      "directoryPath": "s3://example-bucket/PigActivityOutput1",
      "dataFormat": {
        "ref": "MyOutputDataType1"
      },
      "type": "S3DataNode"
    },
    {
      "id": "MyInputDataType1",
      "name": "MyInputDataType1",
      "column": [
        "First STRING",
        "Second STRING",
        "Third STRING",
        "Fourth STRING",
        "Fifth STRING",
        "Sixth STRING",
        "Seventh STRING",
        "Eighth STRING",
        "Ninth STRING",
        "Tenth STRING"
      ],
      "inputRegEx": "^(\\\\S+) (\\\\S+) (\\\\S+) (\\\\S+) (\\\\S+) (\\\\S+) (\\\\S+) (\\\\S+) (\\\\S+) (\\\\S+)",
      "type": "RegEx"
    },
    {
      "id": "MyEmrResource",
      "region": "us-east-1",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "keyPair": "example-keypair",
      "masterInstanceType": "m1.small",
      "enableDebugging": "true",
      "name": "MyEmrResource",
      "actionOnTaskFailure": "continue",
      "type": "EmrCluster"
    },
    {
      "id": "MyOutputDataType4",
      "name": "MyOutputDataType4",
      "column": "one STRING",
      "type": "CSV"
    },
    {
      "id": "MyOutputData4",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "directoryPath": "s3://example-bucket/PigActivityOutput3",
      "name": "MyOutputData4",
      "dataFormat": {
        "ref": "MyOutputDataType4"
      },
      "type": "S3DataNode"
    },
    {
      "id": "MyOutputDataType1",
      "name": "MyOutputDataType1",
      "column": [
        "First STRING",
        "Second STRING",
        "Third STRING",
        "Fourth STRING",
        "Fifth STRING",
        "Sixth STRING",
        "Seventh STRING",
        "Eighth STRING"
      ],
      "columnSeparator": "*",
      "type": "Custom"
    },
    {
      "id": "MyOutputData3",
      "username": "___",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "insertQuery": "insert into #{table} (one) values (?)",
      "name": "MyOutputData3",
      "*password": "___",
      "runsOn": {
        "ref": "MyEmrResource"
      },
      "connectionString": "jdbc:mysql://example-database-instance:3306/example-database",
      "selectQuery": "select * from #{table}",
      "table": "example-table-name",
      "type": "MySqlDataNode"
    },
    {
      "id": "MyOutputDataType2",
      "name": "MyOutputDataType2",
      "column": [
        "Third STRING",
        "Fourth STRING",
        "Fifth STRING",
        "Sixth STRING",
        "Seventh STRING",
        "Eighth STRING"
      ],
      "type": "TSV"
    },
    {
      "id": "MyPigActivity2",
      "scheduleType": "CRON",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "input": {
        "ref": "MyOutputData1"
      },
      "pipelineLogUri": "s3://example-bucket/path",
      "name": "MyPigActivity2",
      "runsOn": {
        "ref": "MyEmrResource"
      },
      "dependsOn": {
        "ref": "MyPigActivity1"
      },
      "type": "PigActivity",
      "script": "B = LIMIT ${input1} 3; ${output1} = FOREACH B GENERATE Third, Fourth, Fifth, Sixth, Seventh, Eighth;",
      "output": {
        "ref": "MyOutputData2"
      },
      "stage": "true"
    },
    {
      "id": "MyEmrResourcePeriod",
      "startDateTime": "2013-05-20T00:00:00",
      "name": "MyEmrResourcePeriod",
      "period": "1 day",
      "type": "Schedule",
      "endDateTime": "2013-05-21T00:00:00"
    },
    {
      "id": "MyPigActivity1",
      "scheduleType": "CRON",
      "schedule": {
        "ref": "MyEmrResourcePeriod"
      },
      "input": {
        "ref": "MyInputData1"
      },
      "pipelineLogUri": "s3://example-bucket/path",
      "scriptUri": "s3://example-bucket/script/pigTestScipt.q",
      "name": "MyPigActivity1",
      "runsOn": {
        "ref": "MyEmrResource"
      },
      "scriptVariable": [
        "column1=First",
        "column2=Second",
        "three=3"
      ],
      "type": "PigActivity",
      "output": {
        "ref": "MyOutputData1"
      },
      "stage": "true"
    }
  ]
}
```

The content of `pigTestScript.q` is as follows\.

```
B = LIMIT ${input1} $three; ${output1} = FOREACH B GENERATE $column1, $column2, Third, Fourth, Fifth, Sixth, Seventh, Eighth;
```

## Syntax<a name="pigactivity-syntax"></a>


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Users must specify a schedule reference to another object to set the dependency execution order for this object\. Users can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), users can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html) | Reference Object, for example, "schedule":\{"ref":"myScheduleId"\} | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| script | The Pig script to run\. | String | 
| scriptUri | The location of the Pig script to run \(for example, s3://scriptLocation\)\. | String | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| runsOn | EMR Cluster on which this PigActivity runs\. | Reference Object, for example, "runsOn":\{"ref":"myEmrClusterId"\} | 
| workerGroup | The worker group\. This is used for routing tasks\. If you provide a runsOn value and workerGroup exists, workerGroup is ignored\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| attemptStatus | The most recently reported status from the remote activity\. | String | 
| attemptTimeout | The timeout for remote work completion\. If set, then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| dependsOn | Specifies the dependency on another runnable object\. | Reference Object, for example, "dependsOn":\{"ref":"myActivityId"\} | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun\. | Enumeration | 
| input | The input data source\. | Reference Object, for example, "input":\{"ref":"myDataNodeId"\} | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | The maximum number attempt retries on failure\. | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, for example, "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, for example, "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object, for example, "onSuccess":\{"ref":"myActionId"\} | 
| output | The output data source\. | Reference Object, for example, "output":\{"ref":"myDataNodeId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, for example, "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The Amazon S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| postActivityTaskConfig | Post\-activity configuration script to be run\. This consists of a URI of the shell script in Amazon S33 and a list of arguments\. | Reference Object, for example, "postActivityTaskConfig":\{"ref":"myShellScriptConfigId"\} | 
| preActivityTaskConfig | Pre\-activity configuration script to be run\. This consists of a URI of the shell script in Amazon S3 and a list of arguments\. | Reference Object, for example, "preActivityTaskConfig":\{"ref":"myShellScriptConfigId"\} | 
| precondition | Optionally define a precondition\. A data node is not marked "READY" until all preconditions have been met\. | Reference Object, for example, "precondition":\{"ref":"myPreconditionId"\} | 
| reportProgressTimeout | The timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| resizeClusterBeforeRunning | Resize the cluster before performing this activity to accommodate DynamoDB data nodes specified as inputs or outputs\. If your activity uses a `DynamoDBDataNode` as either an input or output data node, and if you set the `resizeClusterBeforeRunning` to `TRUE`, AWS Data Pipeline starts using `m3.xlarge` instance types\. This overwrites your instance type choices with `m3.xlarge`, which could increase your monthly costs\.  | Boolean | 
| resizeClusterMaxInstances | A limit on the maximum number of instances that can be requested by the resize algorithm\. | Integer | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of interval or end of the interval\. Time Series Style Scheduling means instances are scheduled at the end of each interval and Cron Style Scheduling means instances are scheduled at the beginning of each interval\. An on\-demand schedule allows you to run a pipeline one time per activation\. This means you do not have to clone or re\-create the pipeline to run it again\. If you use an on\-demand schedule it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use on\-demand pipelines, you simply call the ActivatePipeline operation for each subsequent run\. Values are: cron, ondemand, and timeseries\. | Enumeration | 
| scriptVariable | The arguments to pass to the Pig script\. You can use scriptVariable with script or scriptUri\. | String | 
| stage | Determines whether staging is enabled and allows your Pig script to have access to the staged\-data tables, such as $\{INPUT1\} and $\{OUTPUT1\}\. | Boolean | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, for example, "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain the object failed on\. | Reference Object, for example, "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
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

## See Also<a name="pigactivity-seealso"></a>
+ [ShellCommandActivity](dp-object-shellcommandactivity.md)
+ [EmrActivity](dp-object-emractivity.md)