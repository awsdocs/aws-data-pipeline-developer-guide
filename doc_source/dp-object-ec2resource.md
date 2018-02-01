# Ec2Resource<a name="dp-object-ec2resource"></a>

An EC2 instance that performs the work defined by a pipeline activity\.

## Examples<a name="ec2resource-example"></a>

**EC2\-Classic**

The following example object launches an EC2 instance into EC2\-Classic or a default VPC, with some optional fields set\.

```
{
  "id" : "MyEC2Resource",
  "type" : "Ec2Resource",
  "actionOnTaskFailure" : "terminate",
  "actionOnResourceFailure" : "retryAll",
  "maximumRetries" : "1",
  "instanceType" : "m1.medium",
  "securityGroups" : [
    "test-group",
    "default"
  ],
  "keyPair" : "my-key-pair"
}
```

**EC2\-VPC**

The following example object launches an EC2 instance into a nondefault VPC, with some optional fields set\.

```
{
  "id" : "MyEC2Resource",
  "type" : "Ec2Resource",
  "actionOnTaskFailure" : "terminate",
  "actionOnResourceFailure" : "retryAll",
  "maximumRetries" : "1",
  "instanceType" : "m1.medium",
  "securityGroupIds" : [
    "sg-12345678",
    "sg-12345678"
  ],
  "subnetId": "subnet-12345678",
  "associatePublicIpAddress": "true",
  "keyPair" : "my-key-pair"
}
```

## Syntax<a name="ec2resource-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| resourceRole | The IAM role that controls the resources that the Amazon EC2 instance can access\. | String | 
| role | The IAM role that AWS Data Pipeline uses to create the EC2 instance\. | String | 


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Users must specify a schedule reference to another object to set the dependency execution order for this object\. Users can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), users can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html) | Reference Object, for example "schedule":\{"ref":"myScheduleId"\} | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| actionOnResourceFailure | The action taken after a resource failure for this resource\. Valid values are "retryall" and "retrynone"\. | String | 
| actionOnTaskFailure | The action taken after task failure for this resource\. Valid values are "continue" or "terminate"\. | String | 
| associatePublicIpAddress | Indicates whether to assign a public IP address to the instance\. If the instance is in EC2\-Classic or a default VPC, the default value is true\. Otherwise, the default value is false\. | Boolean | 
| attemptStatus | Most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set, then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| availabilityZone | The Availability Zone in which to launch the Amazon EC2 instance\. | String | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun | Enumeration | 
| httpProxy | The proxy host that clients use to connect to AWS services\. | Reference Object, for example "httpProxy":\{"ref":"myHttpProxyId"\} | 
| imageId | The ID of the AMI to use for the instance\. By default, AWS Data Pipeline uses the PV AMI virtualization type\. The specific AMI IDs used are based on region as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-ec2resource.html) If the `instanceType` specified does not support PV AMIs \(see [Amazon Linux AMI Instance Type Matrix](https://aws.amazon.com/amazon-linux-ami/instance-type-matrix/), specify the ID of an HVM AMI or an error occurs\. For more information about AMI types, see [Linux AMI Virtualization Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html) and [Finding a Linux AMI](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html) in the *Amazon EC2 User Guide for Linux Instances*\.  | String | 
| initTimeout | The amount of time to wait for the resource to start\.  | Period | 
| instanceCount | Deprecated\. | Integer | 
| instanceType | The type of EC2 instance to start\. | String | 
| keyPair | The name of the key pair\. If you launch an EC2 instance without specifying a key pair, you can't log on to it\. | String | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | Maximum number of attempt retries on failure\. | Integer | 
| minInstanceCount | Deprecated\. | Integer | 
| onFail | An action to run when current object fails\. | Reference Object, for example "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object, for example "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when the current object succeeds\. | Reference Object, for example "onSuccess":\{"ref":"myActionId"\} | 
| parent | Parent of the current object from which slots are inherited\. | Reference Object, for example "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The Amazon S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| region |  The code for the region that the EC2 instance should run in\. By default, the instance runs in the same region as the pipeline\. You can run the instance in the same region as a dependent dataset\. | Enumeration | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| runAsUser | The user to run TaskRunner\. | String | 
| runsOn | This field is not allowed on this object\. | Reference Object, for example "runsOn":\{"ref":"myResourceId"\} | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of interval or end of the interval\. Time Series Style Scheduling means that instances are scheduled at the end of each interval and Cron Style Scheduling means that instances are scheduled at the beginning of each interval\. An on\-demand schedule allows you to run a pipeline one time per activation\. You do not have to clone or re\-create the pipeline to run it again\. If you use an on\-demand schedule, it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use on\-demand pipelines, you simply call the ActivatePipeline operation for each subsequent run\. Values are: cron, ondemand, and timeseries\. | Enumeration | 
| securityGroupIds | The IDs of one or more Amazon EC2 security groups to use for the instances in the resource pool\. | String | 
| securityGroups | One or more Amazon EC2 security groups to use for the instances in the resource pool\. | String | 
| spotBidPrice | The maximum amount per hour for your Spot Instance in dollars, which is a decimal value between 0 and 20\.00, exclusive\. | String | 
| subnetId | The ID of the Amazon EC2 subnet in which to start the instance\. | String | 
| terminateAfter | The number of hours after which to terminate the resource\. | Period | 
| useOnDemandOnLastAttempt | On the last attempt to request a Spot Instance, make a request for On\-Demand Instances rather than a Spot Instance\. This ensures that if all previous attempts have failed, the last attempt is not interrupted\.  | Boolean | 
| workerGroup | This field is not allowed on this object\. | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, e\.g\. "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain on which the object failed\. | Reference Object, e\.g\. "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
| emrStepLog | Step logs are available only on Amazon EMR activity attempts\. | String | 
| errorId | The error ID if this object failed\. | String | 
| errorMessage | The error message if this object failed\. | String | 
| errorStackTrace | The error stack trace if this object failed\. | String | 
| @failureReason | The reason for the resource failure\. | String | 
| @finishedTime | The time at which this object finished its execution\. | DateTime | 
| hadoopJobLog | Hadoop job logs available on attempts for Amazon EMR activities\. | String | 
| @healthStatus | The health status of the object that reflects success or failure of the last object instance that reached a terminated state\. | String | 
| @healthStatusFromInstanceId | Id of the last instance object that reached a terminated state\. | String | 
| @healthStatusUpdatedTime | Time at which the health status was updated last time\. | DateTime | 
| hostname | The host name of client that picked up the task attempt\. | String | 
| @lastDeactivatedTime | The time at which this object was last deactivated\. | DateTime | 
| @latestCompletedRunTime | Time the latest run for which the execution completed\. | DateTime | 
| @latestRunTime | Time the latest run for which the execution was scheduled\. | DateTime | 
| @nextRunTime | Time of run to be scheduled next\. | DateTime | 
| reportProgressTime | Most recent time that the remote activity reported progress\. | DateTime | 
| @scheduledEndTime | Schedule end time for the object | DateTime | 
| @scheduledStartTime | Schedule start time for the object | DateTime | 
| @status | The status of this object\. | String | 
| @version | Pipeline version with which the object was created\. | String | 
| @waitingOn | Description of the list of dependencies on which this object is waiting\. | Reference Object, for example "waitingOn":\{"ref":"myRunnableObjectId"\} | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | ID of the pipeline to which this object belongs\. | String | 
| @sphere | The place of an object in the lifecycle\. Component objects give rise to instance objects, which execute attempt objects\. | String | 