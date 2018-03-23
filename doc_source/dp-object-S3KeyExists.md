# S3KeyExists<a name="dp-object-S3KeyExists"></a>

 Checks whether a key exists in an Amazon S3 data node\.

## Example<a name="dp-object-S3KeyExists-example"></a>

The following is an example of this object type\. The precondition will trigger when the key, `s3://mybucket/mykey`, referenced by the `s3Key` parameter, exists\. 

```
{
"id" : "InputReady",
"type" : "S3KeyExists",
"role" : "test-role",
"s3Key" : "s3://mybucket/mykey"
}
```

You can also use `S3KeyExists` as a precondition on the second pipeline that waits for the first pipeline to finish\. To do so:

1. Write a file to Amazon S3 at the end of the first pipeline's completion\.

1. Create an `S3KeyExists` precondition on the second pipeline\.

## Syntax<a name="S3KeyExists-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| role | Specifies the role to be used to execute the precondition\. | String | 
| s3Key | The Amazon S3 key\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| attemptStatus | Most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout before attempting to complete remote work one more time\. If set, then a remote activity that does not complete within the set time after starting is attempted again\. | Period | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun\. | Enumeration | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maximumRetries | Maximum number of attempts that are initiated on failure\. | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, e\.g\. "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, e\.g\. "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object, e\.g\. "onSuccess":\{"ref":"myActionId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| preconditionTimeout | The period from start after which precondition is marked as failed if still not satisfied\. | Period | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and are retried\. | Period | 
| retryDelay | The timeout duration between two successive attempts\. | Period | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, e\.g\. "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain the object failed on\. | Reference Object, e\.g\. "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
| currentRetryCount | Number of times the precondition was tried in this attempt\. | String | 
| emrStepLog | EMR step logs available only on EMR activity attempts | String | 
| errorId | The errorId if this object failed\. | String | 
| errorMessage | The errorMessage if this object failed\. | String | 
| errorStackTrace | The error stack trace if this object failed\. | String | 
| hadoopJobLog | Hadoop job logs available on attempts for EMR\-based activities\. | String | 
| hostname | The host name of client that picked up the task attempt\. | String | 
| lastRetryTime | Last time when the precondition was tried within this attempt\. | String | 
| node | The node for which this precondition is being performed | Reference Object, e\.g\. "node":\{"ref":"myRunnableObjectId"\} | 
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

## See Also<a name="S3KeyExists-seealso"></a>
+ [ShellCommandPrecondition](dp-object-shellcommandprecondition.md)