# EmrCluster<a name="dp-object-emrcluster"></a>

Represents the configuration of an EMR cluster\. This object is used by [EmrActivity](dp-object-emractivity.md) to launch a cluster\.
<a name="emrcluster-schedulers"></a>
**Schedulers**  
Schedulers provide a way to specify resource allocation and job prioritization within a Hadoop cluster\. Administrators or users can choose a scheduler for various classes of users and applications\. A scheduler will possibly use queues to allocate resources to users and applications\. You set up those queues when you create the cluster\. You can then setup priority for certain types of work and user over others\. This provides for efficient use of cluster resources, while allowing more than one user to submit work to the cluster\. There are three types of scheduler available:
+ [FairScheduler](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/FairScheduler.html) — Attempts to schedule resources evenly over a significant period of time\.
+ [CapacityScheduler](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) — Uses queues to allow cluster administrators to assign users to queues of varying priority and resource allocation\. 
+ Default — Used by the cluster, which could be configured by your site\.

**Amazon EMR 2\.x, 3\.x vs\. 4\.x platforms**  
AWS Data Pipeline supports EMR clusters based on release label emr\-4\.0\.0 or later, which requires the use of the `releaseLabel` field for the corresponding `EmrCluster` object\. For previous platforms known as AMI releases, use the `amiVersion` field instead\. If you are using a self\-managed `EmrCluster` object with a release label, use the most current Task Runner\. For more information about TaskRunner, see [Working with Task Runner](dp-using-task-runner.md)\. You can configure all classifications found in the Amazon EMR configuration API\. For a list of all configurations see the Configuring Applications topic in the Amazon EMR Release Guide: [http://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/](http://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/) as well as the [EmrConfiguration](dp-object-emrconfiguration.md) and [Property](dp-object-property.md) object references\. 

## Examples<a name="emrcluster-example"></a>

The following are examples of this object type\.

**Example 1: Launch an EMR cluster using the hadoopVersion field**  <a name="example1"></a>
The following example launches an EMR cluster using AMI version 1\.0 and Hadoop 0\.20\.  

```
{
  "id" : "MyEmrCluster",
  "type" : "EmrCluster",
  "hadoopVersion" : "0.20",
  "keyPair" : "my-key-pair",
  "masterInstanceType" : "m3.xlarge",
  "coreInstanceType" : "m3.xlarge",
  "coreInstanceCount" : "10",
  "taskInstanceType" : "m3.xlarge",
  "taskInstanceCount": "10",
  "bootstrapAction" : ["s3://Region.elasticmapreduce/bootstrap-actions/configure-hadoop,arg1,arg2,arg3","s3://Region.elasticmapreduce/bootstrap-actions/configure-hadoop/configure-other-stuff,arg1,arg2"]
}
```

**Example 1: Launch an EMR cluster with release label emr\-4\.*x* or greater**  
The following example launches an EMR cluster using the newer `releaseLabel` field:  

```
{
  "id" : "MyEmrCluster",
  "type" : "EmrCluster",
  "keyPair" : "my-key-pair",
  "masterInstanceType" : "m3.xlarge",
  "coreInstanceType" : "m3.xlarge",
  "coreInstanceCount" : "10",
  "taskInstanceType" : "m3.xlarge",
  "taskInstanceCount": "10",
  "releaseLabel": "emr-4.1.0",
  "applications": ["spark", "hive", "pig"],
  "configuration": {"ref":"myConfiguration"}  
}
```

**Example 2: Install additional software on your EMR cluster**  <a name="example2"></a>
`EmrCluster` provides the `supportedProducts` field that installs third\-party software on an EMR cluster, for example installing a custom distribution of Hadoop like MapR\. It accepts a comma\-separated list of arguments for the third\-party software to read and act on\. The following example shows how to use the `supportedProducts` field of `EmrCluster` to create a custom MapR M3 edition cluster with Karmasphere Analytics installed, and run an `EmrActivity` object on it\.  

```
{
    "id": "MyEmrActivity",
    "type": "EmrActivity",
    "schedule": {"ref": "ResourcePeriod"},
    "runsOn": {"ref": "MyEmrCluster"},
    "postStepCommand": "echo Ending job >> /mnt/var/log/stepCommand.txt",    
    "preStepCommand": "echo Starting job > /mnt/var/log/stepCommand.txt",
    "step": "/home/hadoop/contrib/streaming/hadoop-streaming.jar,-input,s3n://elasticmapreduce/samples/wordcount/input,-output, \
     hdfs:///output32113/,-mapper,s3n://elasticmapreduce/samples/wordcount/wordSplitter.py,-reducer,aggregate"
  },
  {    
    "id": "MyEmrCluster",
    "type": "EmrCluster",
    "schedule": {"ref": "ResourcePeriod"},
    "supportedProducts": ["mapr,--edition,m3,--version,1.2,--key1,value1","karmasphere-enterprise-utility"],
    "masterInstanceType": "m3.xlarge",
    "taskInstanceType": "m3.xlarge"
}
```

**Example 3: Disable server\-side encryption on 3\.x AMIs**  <a name="example3"></a>
An `EmrCluster` activity with a Hadoop version 2\.*x* created by AWS Data Pipeline enables server\-side encryption by default\. If you would like to disable server\-side encryption, you must specify a bootstrap action in the cluster object definition\.  
The following example creates an `EmrCluster` activity with server\-side encryption disabled:  

```
{  
   "id":"NoSSEEmrCluster",
   "type":"EmrCluster",
   "hadoopVersion":"2.x",
   "keyPair":"my-key-pair",
   "masterInstanceType":"m3.xlarge",
   "coreInstanceType":"m3.large",
   "coreInstanceCount":"10",
   "taskInstanceType":"m3.large",
   "taskInstanceCount":"10",
   "bootstrapAction":["s3://Region.elasticmapreduce/bootstrap-actions/configure-hadoop,-e, fs.s3.enableServerSideEncryption=false"]
}
```

**Example 3: Disable server\-side encryption on 4\.x releases**  <a name="example4"></a>
You must disable server\-side encryption using a `EmrConfiguration` object\.  
The following example creates an `EmrCluster` activity with server\-side encryption disabled:  

```
   {
      "name": "ReleaseLabelCluster",
      "releaseLabel": "emr-4.1.0",
      "applications": ["spark", "hive", "pig"],
      "id": "myResourceId",
      "type": "EmrCluster",
      "configuration": {
        "ref": "disableSSE"
      }
    },
    {
      "name": "disableSSE",
      "id": "disableSSE",
      "type": "EmrConfiguration",
      "classification": "emrfs-site",
      "property": [{
        "ref": "enableServerSideEncryption"
      }
      ]
    },
    {
      "name": "enableServerSideEncryption",
      "id": "enableServerSideEncryption",
      "type": "Property",
      "key": "fs.s3.enableServerSideEncryption",
      "value": "false"
    }
```

**Example Configure Hadoop KMS Access Control Lists \(ACLs\) and Create Encryption Zones in HDFS**  
The following objects will create ACLs for Hadoop KMS and create encryption zones and corresponding encryption keys in HDFS:  

```
{
      "name": "kmsAcls",
      "id": "kmsAcls",
      "type": "EmrConfiguration",
      "classification": "hadoop-kms-acls",
      "property": [
        {"ref":"kmsBlacklist"},
        {"ref":"kmsAcl"}
      ]
    },
    {
      "name": "hdfsEncryptionZone",
      "id": "hdfsEncryptionZone",
      "type": "EmrConfiguration",
      "classification": "hdfs-encryption-zones",
      "property": [
        {"ref":"hdfsPath1"},
        {"ref":"hdfsPath2"}
      ]
    },
    {
      "name": "kmsBlacklist",
      "id": "kmsBlacklist",
      "type": "Property",
      "key": "hadoop.kms.blacklist.CREATE",
      "value": "foo,myBannedUser"
    },
    {
      "name": "kmsAcl",
      "id": "kmsAcl",
      "type": "Property",
      "key": "hadoop.kms.acl.ROLLOVER",
      "value": "myAllowedUser"
    },
    {
      "name": "hdfsPath1",
      "id": "hdfsPath1",
      "type": "Property",
      "key": "/myHDFSPath1",
      "value": "path1_key"
    },
    {
      "name": "hdfsPath2",
      "id": "hdfsPath2",
      "type": "Property",
      "key": "/myHDFSPath2",
      "value": "path2_key"
    }
```

**Example 4: Specify custom IAM roles**  
By default, AWS Data Pipeline passes `DataPipelineDefaultRole` as the EMR service role and `DataPipelineDefaultResourceRole` as the EC2 instance profile to create resources on your behalf\. However, you can create a custom EMR service role and a custom instance profile and use them instead\. AWS Data Pipeline should have sufficient permissions to create clusters using the custom role, and you must add AWS Data Pipeline as a trusted entity\.  
The following example object specifies custom roles for the EMR cluster:  

```
{  
   "id":"MyEmrCluster",
   "type":"EmrCluster",
   "hadoopVersion":"2.x",
   "keyPair":"my-key-pair",
   "masterInstanceType":"m3.xlarge",
   "coreInstanceType":"m3.large",
   "coreInstanceCount":"10",
   "taskInstanceType":"m3.large",
   "taskInstanceCount":"10",
   "role":"emrServiceRole",
   "resourceRole":"emrInstanceProfile"
}
```

**Example Using EmrCluster Resource in AWS SDK for Java**  
The following shows how to use an EmrCluster and EmrActivity to create an Amazon EMR 4\.x cluster to run a Spark step using the Java SDK:  

```
public class dataPipelineEmr4 {

  public static void main(String[] args) {
    
	AWSCredentials credentials = null;
	credentials = new ProfileCredentialsProvider("/path/to/AwsCredentials.properties","default").getCredentials();
	DataPipelineClient dp = new DataPipelineClient(credentials);
	CreatePipelineRequest createPipeline = new CreatePipelineRequest().withName("EMR4SDK").withUniqueId("unique");
	CreatePipelineResult createPipelineResult = dp.createPipeline(createPipeline);
	String pipelineId = createPipelineResult.getPipelineId();
    
	PipelineObject emrCluster = new PipelineObject()
	    .withName("EmrClusterObj")
	    .withId("EmrClusterObj")
	    .withFields(
			new Field().withKey("releaseLabel").withStringValue("emr-4.1.0"),
			new Field().withKey("coreInstanceCount").withStringValue("3"),
			new Field().withKey("applications").withStringValue("spark"),
			new Field().withKey("applications").withStringValue("Presto-Sandbox"),
			new Field().withKey("type").withStringValue("EmrCluster"),
			new Field().withKey("keyPair").withStringValue("myKeyName"),
			new Field().withKey("masterInstanceType").withStringValue("m3.xlarge"),
			new Field().withKey("coreInstanceType").withStringValue("m3.xlarge")        
			);
  
	PipelineObject emrActivity = new PipelineObject()
	    .withName("EmrActivityObj")
	    .withId("EmrActivityObj")
	    .withFields(
			new Field().withKey("step").withStringValue("command-runner.jar,spark-submit,--executor-memory,1g,--class,org.apache.spark.examples.SparkPi,/usr/lib/spark/lib/spark-examples.jar,10"),
			new Field().withKey("runsOn").withRefValue("EmrClusterObj"),
			new Field().withKey("type").withStringValue("EmrActivity")
			);
      
	PipelineObject schedule = new PipelineObject()
	    .withName("Every 15 Minutes")
	    .withId("DefaultSchedule")
	    .withFields(
			new Field().withKey("type").withStringValue("Schedule"),
			new Field().withKey("period").withStringValue("15 Minutes"),
			new Field().withKey("startAt").withStringValue("FIRST_ACTIVATION_DATE_TIME")
			);
      
	PipelineObject defaultObject = new PipelineObject()
	    .withName("Default")
	    .withId("Default")
	    .withFields(
			new Field().withKey("failureAndRerunMode").withStringValue("CASCADE"),
			new Field().withKey("schedule").withRefValue("DefaultSchedule"),
			new Field().withKey("resourceRole").withStringValue("DataPipelineDefaultResourceRole"),
			new Field().withKey("role").withStringValue("DataPipelineDefaultRole"),
			new Field().withKey("pipelineLogUri").withStringValue("s3://myLogUri"),
			new Field().withKey("scheduleType").withStringValue("cron")
			);     
      
	List<PipelineObject> pipelineObjects = new ArrayList<PipelineObject>();
    
	pipelineObjects.add(emrActivity);
	pipelineObjects.add(emrCluster);
	pipelineObjects.add(defaultObject);
	pipelineObjects.add(schedule);
    
	PutPipelineDefinitionRequest putPipelineDefintion = new PutPipelineDefinitionRequest()
	    .withPipelineId(pipelineId)
	    .withPipelineObjects(pipelineObjects);
    
	PutPipelineDefinitionResult putPipelineResult = dp.putPipelineDefinition(putPipelineDefintion);
	System.out.println(putPipelineResult);
    
	ActivatePipelineRequest activatePipelineReq = new ActivatePipelineRequest()
	    .withPipelineId(pipelineId);
	ActivatePipelineResult activatePipelineRes = dp.activatePipeline(activatePipelineReq);
	
      System.out.println(activatePipelineRes);
      System.out.println(pipelineId);
    
    }

}
```

When you create a custom IAM role, carefully consider the minimum permissions necessary for your cluster to perform its work\. Be sure to grant access to required resources, such as files in Amazon S3 or data in Amazon RDS, Amazon Redshift or DynamoDB\.

If you wish to set `visibleToAllUsers` to False, your role must have the proper permissions to do so\. Note that `DataPipelineDefaultRole` does not have these permissions\. You must either provide a union of the `DefaultDataPipelineResourceRole` and `DataPipelineDefaultRole` roles as the `EmrCluster` object role or create your own role for this purpose\.

## Syntax<a name="emrcluster-syntax"></a>


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule | This object is invoked within the execution of a schedule interval\. Users must specify a schedule reference to another object to set the dependency execution order for this object\. Users can satisfy this requirement by explicitly setting a schedule on the object, for example, by specifying "schedule": \{"ref": "DefaultSchedule"\}\. In most cases, it is better to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. Or, if the pipeline has a tree of schedules \(schedules within the master schedule\), users can create a parent object that has a schedule reference\. For more information about example optional schedule configurations, see [http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html) | Reference Object, for example "schedule":\{"ref":"myScheduleId"\} | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| actionOnResourceFailure | The action taken after a resource failure for this resource\. Valid values are "retryall", which retries all tasks to the cluster for the specified duration, and "retrynone"\. | String | 
| actionOnTaskFailure | The action taken after task failure for this resource\. Valid values are "continue", meaning do not terminate the cluster, and "terminate\." | String | 
| additionalMasterSecurityGroupIds | The identifier of additional master security groups of the EMR cluster, which follows the form sg\-01XXXX6a\. For more information, see [Amazon EMR Additional Security Groups](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-additional-sec-groups.html) in the Amazon EMR Management Guide\. | String | 
| additionalSlaveSecurityGroupIds | The identifier of additional slave security groups of the EMR cluster, which follows the form sg\-01XXXX6a\. | String | 
| amiVersion | The Amazon Machine Image \(AMI\) version that Amazon EMR uses to install the cluster nodes\. For more information, see the [Amazon EMR Management Guide](http://docs.aws.amazon.com/emr/latest/ManagementGuide/)\. | String | 
| applications | Applications to install in the cluster with comma\-separated arguments\. By default, Hive and Pig are installed\. This parameter is applicable only for releaseLabel emr\-4\.0\.0 and later\. | String | 
| attemptStatus | The most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set, then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| availabilityZone | The Availability Zone in which to run the cluster\. | String | 
| bootstrapAction | An action to run when the cluster starts\. You can specify comma\-separated arguments\. To specify multiple actions, up to 255, add multiple bootstrapAction fields\. The default behavior is to start the cluster without any bootstrap actions\. | String | 
| configuration | Configuration for the EMR cluster\. This parameter is applicable only for releaseLabel emr\-4\.0\.0 and later\. | Reference Object, for example "configuration":\{"ref":"myEmrConfigurationId"\} | 
| coreInstanceBidPrice | The maximum dollar amount per hour for your Spot Instance, which is a decimal value between 0 and 20\.00, exclusive\. Setting this value enables Spot Instances for the EMR cluster core nodes\. Must be used together with coreInstanceCount\. | String | 
| coreInstanceCount | The number of core nodes to use for the cluster\. | Integer | 
| coreInstanceType | The type of EC2 instance to use for core nodes\. The default value is m1\.small\. | String | 
| emrManagedMasterSecurityGroupId | The identifier of the master security group of the EMR cluster, which follows the form sg\-01XXXX6a\. For more information, see [Configure Security Groups](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-security-groups.html) in the Amazon EMR Management Guide\. | String | 
| emrManagedSlaveSecurityGroupId | The identifier of the slave security group of the EMR cluster, which follows the form sg\-01XXXX6a\. | String | 
| enableDebugging | Enables debugging on the EMR cluster\. | String | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun\. | Enumeration | 
| hadoopSchedulerType | The scheduler type of the cluster\. Valid types are: PARALLEL\_FAIR\_SCHEDULING, PARALLEL\_CAPACITY\_SCHEDULING, and DEFAULT\_SCHEDULER\. | Enumeration | 
| httpProxy | The proxy host that clients use to connect to AWS services\. | Reference Object, for example "httpProxy":\{"ref":"myHttpProxyId"\} | 
| initTimeout | The amount of time to wait for the resource to start\.  | Period | 
| keyPair | The Amazon EC2 key pair to use to log on to the master node of the EMR cluster\. | String | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| masterInstanceBidPrice | The maximum dollar amount for your Spot Instance, which is a decimal value between 0 and 20\.00, exclusive\. Setting this value enables Spot Instances for the EMR cluster master node\. | String | 
| masterInstanceType | The type of EC2 instance to use for the master node\. The default value is m1\.small\. | String | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | Maximum number attempt retries on failure\. | Integer | 
| onFail | An action to run when the current object fails\. | Reference Object, for example "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or is still not completed\. | Reference Object, for example "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when the current object succeeds\. | Reference Object, for example "onSuccess":\{"ref":"myActionId"\} | 
| parent | Parent of the current object from which slots are inherited\. | Reference Object, for example "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The Amazon S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| region | The code for the region that the EMR cluster should run in\. By default, the cluster runs in the same region as the pipeline\. You can run the cluster in the same region as a dependent dataset\.  | Enumeration | 
| releaseLabel | release label for the EMR cluster | String | 
| reportProgressTimeout | Timeout for remote work successive calls to reportProgress\. If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\. | Period | 
| resourceRole | The IAM role that AWS Data Pipeline uses to create the EMR cluster\. The default role is DataPipelineDefaultRole\.  | String | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| role | The IAM role passed to Amazon EMR to create EC2 nodes\. | String | 
| runsOn | This field is not allowed on this object\. | Reference Object, for example "runsOn":\{"ref":"myResourceId"\} | 
| scheduleType | Schedule type allows you to specify whether the objects in your pipeline definition should be scheduled at the beginning of interval or end of the interval\. Time Series Style Scheduling means that instances are scheduled at the end of each interval and Cron Style Scheduling means that instances are scheduled at the beginning of each interval\. An on\-demand schedule allows you to run a pipeline one time per activation\. You do not have to clone or re\-create the pipeline to run it again\. If you use an on\-demand schedule, it must be specified in the default object and must be the only scheduleType specified for objects in the pipeline\. To use on\-demand pipelines, you simply call the ActivatePipeline operation for each subsequent run\. Values are: cron, ondemand, and timeseries\. | Enumeration | 
| subnetId | The identifier of the subnet to launch the cluster into\. | String | 
| supportedProducts | A parameter that installs third\-party software on an EMR cluster, for example installing a third\-party distribution of Hadoop\. | String | 
| taskInstanceBidPrice | The maximum dollar amount for your Spot Instance\. A decimal value between 0 and 20\.00, exclusive\. Setting this value enables Spot Instances for the EMR cluster task nodes\. | String | 
| taskInstanceCount | The number of task nodes to use for the cluster\. | Integer | 
| taskInstanceType | The type of EC2 instance to use for task nodes\. | String | 
| terminateAfter | Terminate the resource after these many hours\. | Period | 
| useOnDemandOnLastAttempt | On the last attempt to request a resource, make a request for On\-Demand Instances rather than Spot Instances\. This ensures that if all previous attempts have failed, the last attempt is not interrupted\.  | Boolean | 
| workerGroup | Field not allowed on this object | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object, for example "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain on which the object failed\. | Reference Object, for example "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
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
| @scheduledEndTime | Schedule end time for object | DateTime | 
| @scheduledStartTime | Schedule start time for object | DateTime | 
| @status | The status of this object\. | String | 
| @version | Pipeline version with which the object was created\. | String | 
| @waitingOn | Description of list of dependencies on which this object is waiting\. | Reference Object, for example "waitingOn":\{"ref":"myRunnableObjectId"\} | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | ID of the pipeline to which this object belongs\. | String | 
| @sphere | The place of an object in the lifecycle\. Component objects give rise to instance objects, which execute attempt objects\. | String | 

## See Also<a name="emrcluster-seealso"></a>
+ [EmrActivity](dp-object-emractivity.md)