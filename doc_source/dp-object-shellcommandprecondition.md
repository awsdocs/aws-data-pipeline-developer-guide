# ShellCommandPrecondition<a name="dp-object-shellcommandprecondition"></a>

 A Unix/Linux shell command that can be run as a precondition\. 

## Example<a name="shellcommandprecondition-example"></a>

The following is an example of this object type\.

```
{
  "id" : "VerifyDataReadiness",
  "type" : "ShellCommandPrecondition",
  "command" : "perl check-data-ready.pl"
}
```

## Syntax<a name="shellcommandprecondition-syntax"></a>


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| command | The command to run\. This value and any associated parameters must function in the environment from which you are running the Task Runner\. | String | 
| scriptUri | An Amazon S3 URI path for a file to download and run as a shell command\. Only one scriptUri or command field should be present\. scriptUri cannot use parameters, use command instead\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| attemptStatus | Most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun | Enumeration | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maximumRetries | Maximum number attempt retries on failure | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, e\.g\. "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, e\.g\. "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object, e\.g\. "onSuccess":\{"ref":"myActionId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| preconditionTimeout | The period from start after which precondition is marked as failed if still not satisfied | Period | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| scriptArgument | Argument to be passed to shell script | String | 
| stderr | The Amazon S3 path that receives redirected system error messages from the command\. If you use the runsOn field, this must be an Amazon S3 path because of the transitory nature of the resource running your activity\. However, if you specify the workerGroup field, a local file path is permitted\. | String | 
| stdout | The Amazon S3 path that receives redirected output from the command\. If you use the runsOn field, this must be an Amazon S3 path because of the transitory nature of the resource running your activity\. However, if you specify the workerGroup field, a local file path is permitted\. | String | 


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
| hadoopJobLog | Hadoop job logs available on attempts for EMR\-based activities\. | String | 
| hostname | The host name of client that picked up the task attempt\. | String | 
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

## See Also<a name="shellcommandprecondition-seealso"></a>
+ [ShellCommandActivity](dp-object-shellcommandactivity.md)
+ [Exists](dp-object-exists.md)