# EmrCluster<a name="dp-object-emrcluster"></a>

Represents the configuration of an Amazon EMR cluster\. This object is used by [EmrActivity](dp-object-emractivity.md) to launch a cluster\.

**Topics**
+ [Examples](emrcluster-example.md)
+ [Syntax](#emrcluster-syntax)
+ [See Also](#emrcluster-seealso)
<a name="emrcluster-schedulers"></a>
**Schedulers**  
Schedulers provide a way to specify resource allocation and job prioritization within a Hadoop cluster\. Administrators or users can choose a scheduler for various classes of users and applications\. A scheduler could use queues to allocate resources to users and applications\. You set up those queues when you create the cluster\. You can then set up priority for certain types of work and user over others\. This provides for efficient use of cluster resources, while allowing more than one user to submit work to the cluster\. There are three types of scheduler available:
+ [FairScheduler](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/FairScheduler.html) — Attempts to schedule resources evenly over a significant period of time\.
+ [CapacityScheduler](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) — Uses queues to allow cluster administrators to assign users to queues of varying priority and resource allocation\. 
+ Default — Used by the cluster, which could be configured by your site\.

**Amazon EMR 2\.x, 3\.x vs\. 4\.x platforms**  
AWS Data Pipeline supports Amazon EMR clusters based on release label emr\-4\.0\.0 or later, which requires the use of the `releaseLabel` field for the corresponding `EmrCluster` object\. For previous platforms known as AMI releases, use the `amiVersion` field instead\. 

If you are using a self\-managed `EmrCluster` object with a release label, use the most current Task Runner\. For more information about Task Runner, see [Working with Task Runner](dp-using-task-runner.md)\. You can configure property values for all Amazon EMR configuration classifications\. For more information, see [Configuring Applications](http://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/emr-configure-apps.html) in the *Amazon EMR Release Guide*, and the [EmrConfiguration](dp-object-emrconfiguration.md), and [Property](dp-object-property.md) object references\. 

**Amazon EMR permissions**  
When you create a custom IAM role, carefully consider the minimum permissions necessary for your cluster to perform its work\. Be sure to grant access to required resources, such as files in Amazon S3 or data in Amazon RDS, Amazon Redshift or DynamoDB\. If you wish to set `visibleToAllUsers` to False, your role must have the proper permissions to do so\. Note that `DataPipelineDefaultRole` does not have these permissions\. You must either provide a union of the `DefaultDataPipelineResourceRole` and `DataPipelineDefaultRole` roles as the `EmrCluster` object role or create your own role for this purpose\.

## Syntax<a name="emrcluster-syntax"></a>


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Specify a schedule reference to another object to set the dependency execution order for this object\. You can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), you can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html) | Reference Object, for example, "schedule":\{"ref":"myScheduleId"\} | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| actionOnResourceFailure | The action taken after a resource failure for this resource\. Valid values are "retryall", which retries all tasks to the cluster for the specified duration, and "retrynone"\. | String | 
| actionOnTaskFailure | The action taken after task failure for this resource\. Valid values are "continue", meaning do not terminate the cluster, and "terminate\." | String | 
| additionalMasterSecurityGroupIds | The identifier of additional master security groups of the EMR cluster, which follows the form sg\-01XXXX6a\. For more information, see [Amazon EMR Additional Security Groups](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-additional-sec-groups.html) in the Amazon EMR Management Guide\. | String | 
| additionalSlaveSecurityGroupIds | The identifier of additional slave security groups of the EMR cluster, which follows the form sg\-01XXXX6a\. | String | 
| amiVersion | The Amazon Machine Image \(AMI\) version that Amazon EMR uses to install the cluster nodes\. For more information, see the [Amazon EMR Management Guide](https://docs.aws.amazon.com/emr/latest/ManagementGuide/)\. | String | 
| applications | Applications to install in the cluster with comma\-separated arguments\. By default, Hive and Pig are installed\. This parameter is applicable only for Amazon EMR version 4\.0 and later\. | String | 
| attemptStatus | The most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set, then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| availabilityZone | The Availability Zone in which to run the cluster\. | String | 
| bootstrapAction | An action to run when the cluster starts\. You can specify comma\-separated arguments\. To specify multiple actions, up to 255, add multiple bootstrapAction fields\. The default behavior is to start the cluster without any bootstrap actions\. | String | 
| configuration | Configuration for the Amazon EMR cluster\. This parameter is applicable only for Amazon EMR version 4\.0 and later\. | Reference Object, for example, "configuration":\{"ref":"myEmrConfigurationId"\} | 
| coreInstanceBidPrice | The maximum Spot price your are willing to pay for Amazon EC2 instances\. If a bid price is specified, Amazon EMR uses Spot Instances for the instance group\. Specified in USD\. Alternatively, a value of OnDemandPrice indicates that the maximum Spot price is set equal to the On\-Demand price\. Must be used together with coreInstanceCount\. | String | 
| coreInstanceCount | The number of core nodes to use for the cluster\. | Integer | 
| coreInstanceType | The type of Amazon EC2 instance to use for core nodes\. See [Supported Amazon EC2 Instances for Amazon EMR Clusters ](dp-emr-supported-instance-types.md)\. | String | 
| coreGroupConfiguration | The configuration for the Amazon EMR cluster core instance group\. This parameter is applicable only for Amazon EMR version 4\.0 and later\. | Reference Object, for example “configuration”: \{“ref”: “myEmrConfigurationId”\} | 
| coreEbsConfiguration | The configuration for Amazon EBS volumes that will be attached to each of the core nodes in the core group in the Amazon EMR cluster\. For more information, see [Instance Types That Support EBS Optimization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html) in the Amazon EC2 User Guide for Linux Instances\. | Reference Object, for example “coreEbsConfiguration”: \{“ref”: “myEbsConfiguration”\} | 
| EbsBlockDeviceConfig |  The configuration of a requested Amazon EBS block device associated with the instance group\. Includes a specified number of volumes that will be associated with each instance in the instance group\. Includes `volumesPerInstance` and `volumeSpecification`, where:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-emrcluster.html)  | Reference Object, for example “EbsBlockDeviceConfig”: \{“ref”: “myEbsBlockDeviceConfig”\} | 
| emrManagedMasterSecurityGroupId | The identifier of the master security group of the Amazon EMR cluster, which follows the form of sg\-01XXXX6a\. For more information, see [Configure Security Groups](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-security-groups.html) in the Amazon EMR Management Guide\. | String | 
| emrManagedSlaveSecurityGroupId | The identifier of the slave security group of the Amazon EMR cluster, which follows the form sg\-01XXXX6a\. | String | 
| enableDebugging | Enables debugging on the Amazon EMR cluster\. | String | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun\. | Enumeration | 
| hadoopSchedulerType | The scheduler type of the cluster\. Valid types are: PARALLEL\_FAIR\_SCHEDULING, PARALLEL\_CAPACITY\_SCHEDULING, and DEFAULT\_SCHEDULER\. | Enumeration | 
| httpProxy | The proxy host that clients use to connect to AWS services\. | Reference Object, for example, "httpProxy":\{"ref":"myHttpProxyId"\} | 
| initTimeout | The amount of time to wait for the resource to start\.  | Period | 
| keyPair | The Amazon EC2 key pair to use to log on to the master node of the Amazon EMR cluster\. | String | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| masterInstanceBidPrice | The maximum Spot price your are willing to pay for Amazon EC2 instances\. It is a decimal value between 0 and 20\.00, exclusive\. Specified in USD\. Setting this value enables Spot Instances for the Amazon EMR cluster master node\. If a bid price is specified, Amazon EMR uses Spot Instances for the instance group\. Alternatively, a value of OnDemandPrice indicates that the maximum Spot price is set equal to the On\-Demand price\. | String | 
| masterInstanceType | The type of Amazon EC2 instance to use for the master node\. See [Supported Amazon EC2 Instances for Amazon EMR Clusters ](dp-emr-supported-instance-types.md)\. | String | 
| masterGroupConfiguration | The configuration for the Amazon EMR cluster master instance group\. This parameter is applicable only for Amazon EMR version 4\.0 and later\. | Reference Object, for example “configuration”: \{“ref”: “myEmrConfigurationId”\} | 
| masterEbsConfiguration | The configuration for Amazon EBS volumes that will be attached to each of the master nodes in the master group in the Amazon EMR cluster\. For more information, see [Instance Types That Support EBS Optimization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html) in the Amazon EC2 User Guide for Linux Instances\. | Reference Object, for example “masterEbsConfiguration”: \{“ref”: “myEbsConfiguration”\} | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | Maximum number attempt retries on failure\. | Integer | 
| onFail | An action to run when the current object fails\. | Reference Object, for example, "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or is still not completed\. | Reference Object, for example, "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when the current object succeeds\. | Reference Object, for example, "onSuccess":\{"ref":"myActionId"\} | 
| parent | Parent of the current object from which slots are inherited\. | Reference Object, for example\. "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The Amazon S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| region | The code for the region that the Amazon EMR cluster should run in\. By default, the cluster runs in the same region as the pipeline\. You can run the cluster in the same region as a dependent dataset\.  | Enumeration | 
| releaseLabel | Release label for the EMR cluster\. | String | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| resourceRole | The IAM role that AWS Data Pipeline uses to create the Amazon EMR cluster\. The default role is DataPipelineDefaultRole\.  | String | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| role | The IAM role passed to Amazon EMR to create EC2 nodes\. | String | 
| runsOn | This field is not allowed on this object\. | Reference Object, for example, "runsOn":\{"ref":"myResourceId"\} | 
| serviceAccessSecurityGroupId | The identifier for the service access security group of the Amazon EMR cluster\.  | String\. It follows the form of sg\-01XXXX6a, for example, sg\-1234abcd\. | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of the interval, or end of the interval\. Values are: cron, ondemand, and timeseries\. The timeseries scheduling means that instances are scheduled at the end of each interval\. The cron scheduling means that instances are scheduled at the beginning of each interval\. An ondemand schedule allows you to run a pipeline one time per activation\. You do not have to clone or re\-create the pipeline to run it again\. If you use an ondemand schedule, it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use ondemand pipelines, call the ActivatePipeline operation for each subsequent run\. | Enumeration | 
| subnetId | The identifier of the subnet into which to launch the Amazon EMR cluster\. | String | 
| supportedProducts | A parameter that installs third\-party software on an Amazon EMR cluster, for example, a third\-party distribution of Hadoop\. | String | 
| taskInstanceBidPrice | The maximum Spot price your are willing to pay for EC2 instances\. A decimal value between 0 and 20\.00, exclusive\. Specified in USD\. If a bid price is specified, Amazon EMR uses Spot Instances for the instance group\. Alternatively, a value of OnDemandPrice indicates that the maximum Spot price is set equal to the On\-Demand price\. Setting this value enables Spot Instances for the Amazon EMR cluster task nodes\. | String | 
| taskInstanceCount | The number of task nodes to use for the Amazon EMR cluster\. | Integer | 
| taskInstanceType | The type of Amazon EC2 instance to use for task nodes\. | String | 
| taskGroupConfiguration | The configuration for the Amazon EMR cluster task instance group\. This parameter is applicable only for Amazon EMR version 4\.0 and later\.  | Reference Object, for example “configuration”: \{“ref”: “myEmrConfigurationId”\} | 
| taskEbsConfiguration | The configuration for Amazon EBS volumes that will be attached to each of the task nodes in the task group in the Amazon EMR cluster\. For more information, see [Instance Types That Support EBS Optimization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html) in the Amazon EC2 User Guide for Linux Instances\. | Reference Object, for example “taskEbsConfiguration”: \{“ref”: “myEbsConfiguration”\} | 
| terminateAfter | Terminate the resource after these many hours\. | Integer | 
| VolumeSpecification |   The Amazon EBS volume specifications, such as volume type, IOPS, and size in Gigibytes \(GiB\) that will be requested for the Amazon EBS volume attached to an Amazon EC2 instance in the Amazon EMR cluster\. The node can be a core, master or task node\.  The `VolumeSpecification` includes: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-emrcluster.html)  | Reference Object, for example “VolumeSpecification”: \{“ref”: “myVolumeSpecification”\} | 
| useOnDemandOnLastAttempt | On the last attempt to request a resource, make a request for On\-Demand Instances rather than Spot Instances\. This ensures that if all previous attempts have failed, the last attempt is not interrupted\.  | Boolean | 
| workerGroup | Field not allowed on this object\. | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, for example, "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain on which the object failed\. | Reference Object, for example, "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
| emrStepLog | Step logs available only on Amazon EMR activity attempts\. | String | 
| errorId | The error ID if this object failed\. | String | 
| errorMessage | The error message if this object failed\. | String | 
| errorStackTrace | The error stack trace if this object failed\. | String | 
| @failureReason | The reason for the resource failure\. | String | 
| @finishedTime | The time at which this object finished its execution\. | DateTime | 
| hadoopJobLog | Hadoop job logs available on attempts for Amazon EMR activities\. | String | 
| @healthStatus | The health status of the object that reflects success or failure of the last object instance that reached a terminated state\. | String | 
| @healthStatusFromInstanceId | ID of the last instance object that reached a terminated state\. | String | 
| @healthStatusUpdatedTime | Time at which the health status was updated last time\. | DateTime | 
| hostname | The host name of client that picked up the task attempt\. | String | 
| @lastDeactivatedTime | The time at which this object was last deactivated\. | DateTime | 
| @latestCompletedRunTime | Time the latest run for which the execution completed\. | DateTime | 
| @latestRunTime | Time the latest run for which the execution was scheduled\. | DateTime | 
| @nextRunTime | Time of run to be scheduled next\. | DateTime | 
| reportProgressTime | Most recent time that remote activity reported progress\. | DateTime | 
| @scheduledEndTime | Schedule end time for object\. | DateTime | 
| @scheduledStartTime | Schedule start time for object\. | DateTime | 
| @status | The status of this object\. | String | 
| @version | Pipeline version with which the object was created\. | String | 
| @waitingOn | Description of the list of dependencies on which this object is waiting\. | Reference Object, for example, "waitingOn":\{"ref":"myRunnableObjectId"\} | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | ID of the pipeline to which this object belongs\. | String | 
| @sphere | The place of an object in the lifecycle\. Component objects give rise to instance objects, which execute attempt objects\. | String | 

## See Also<a name="emrcluster-seealso"></a>
+ [EmrActivity](dp-object-emractivity.md)