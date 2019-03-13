# RedshiftCopyActivity<a name="dp-object-redshiftcopyactivity"></a>

Copies data from DynamoDB or Amazon S3 to Amazon Redshift\. You can load data into a new table, or easily merge data into an existing table\.

Here is an overview of a use case in which to use `RedshiftCopyActivity`:

1. Start by using AWS Data Pipeline to stage your data in Amazon S3\. 

1. Use `RedshiftCopyActivity` to move the data from Amazon RDS and Amazon EMR to Amazon Redshift\.

   This lets you load your data into Amazon Redshift where you can analyze it\.

1. Use [SqlActivity](dp-object-sqlactivity.md) to perform SQL queries on the data that you've loaded into Amazon Redshift\.

 In addition, `RedshiftCopyActivity` let's you work with an `S3DataNode`, since it supports a manifest file\. For more information, see [S3DataNode](dp-object-s3datanode.md)\.

## Example<a name="redshiftcopyactivity-example"></a>

The following is an example of this object type\. 

To ensure formats conversion, this example uses [EMPTYASNULL](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-conversion.html#copy-emptyasnull) and [IGNOREBLANKLINES](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-conversion.html#copy-ignoreblanklines) special conversion parameters in `commandOptions`\. For information, see [Data Conversion Parameters](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-conversion.html) in the *Amazon Redshift Database Developer Guide*\.

```
{
  "id" : "S3ToRedshiftCopyActivity",
  "type" : "RedshiftCopyActivity",
  "input" : { "ref": "MyS3DataNode" },
  "output" : { "ref": "MyRedshiftDataNode" },
  "insertMode" : "KEEP_EXISTING",
  "schedule" : { "ref": "Hour" },
  "runsOn" : { "ref": "MyEc2Resource" },
  "commandOptions": ["EMPTYASNULL", "IGNOREBLANKLINES"]
}
```

The following example pipeline definition shows an activity that uses the `APPEND` insert mode:

```
{
  "objects": [
    {
      "id": "CSVId1",
      "name": "DefaultCSV1",
      "type": "CSV"
    },
    {
      "id": "RedshiftDatabaseId1",
      "databaseName": "dbname",
      "username": "user",
      "name": "DefaultRedshiftDatabase1",
      "*password": "password",
      "type": "RedshiftDatabase",
      "clusterId": "redshiftclusterId"
    },
    {
      "id": "Default",
      "scheduleType": "timeseries",
      "failureAndRerunMode": "CASCADE",
      "name": "Default",
      "role": "DataPipelineDefaultRole",
      "resourceRole": "DataPipelineDefaultResourceRole"
    },
    {
      "id": "RedshiftDataNodeId1",
      "schedule": {
        "ref": "ScheduleId1"
      },
      "tableName": "orders",
      "name": "DefaultRedshiftDataNode1",
      "createTableSql": "create table StructuredLogs (requestBeginTime CHAR(30) PRIMARY KEY DISTKEY SORTKEY, requestEndTime CHAR(30), hostname CHAR(100), requestDate varchar(20));",
      "type": "RedshiftDataNode",
      "database": {
        "ref": "RedshiftDatabaseId1"
      }
    },
    {
      "id": "Ec2ResourceId1",
      "schedule": {
        "ref": "ScheduleId1"
      },
      "securityGroups": "MySecurityGroup",
      "name": "DefaultEc2Resource1",
      "role": "DataPipelineDefaultRole",
      "logUri": "s3://myLogs",
      "resourceRole": "DataPipelineDefaultResourceRole",
      "type": "Ec2Resource"
    },
    {
      "id": "ScheduleId1",
      "startDateTime": "yyyy-mm-ddT00:00:00",
      "name": "DefaultSchedule1",
      "type": "Schedule",
      "period": "period",
      "endDateTime": "yyyy-mm-ddT00:00:00"
    },
    {
      "id": "S3DataNodeId1",
      "schedule": {
        "ref": "ScheduleId1"
      },
      "filePath": "s3://datapipeline-us-east-1/samples/hive-ads-samples.csv",
      "name": "DefaultS3DataNode1",
      "dataFormat": {
        "ref": "CSVId1"
      },
      "type": "S3DataNode"
    },
    {
      "id": "RedshiftCopyActivityId1",
      "input": {
        "ref": "S3DataNodeId1"
      },
      "schedule": {
        "ref": "ScheduleId1"
      },
      "insertMode": "APPEND",
      "name": "DefaultRedshiftCopyActivity1",
      "runsOn": {
        "ref": "Ec2ResourceId1"
      },
      "type": "RedshiftCopyActivity",
      "output": {
        "ref": "RedshiftDataNodeId1"
      }
    }
  ]
}
```

`APPEND` operation adds items to a table regardless of the primary or sort keys\. For example, if you have the following table, you can append a record with the same ID and user value\.

```
ID(PK)     USER
1          aaa
2          bbb
```

You can append a record with the same ID and user value:

```
ID(PK)     USER
1          aaa
2          bbb
1          aaa
```

**Note**  
If an `APPEND` operation is interrupted and retried, the resulting rerun pipeline potentially appends from the beginning\. This may cause further duplication, so you should be aware of this behavior, especially if you have any logic that counts the number of rows\.

For a tutorial, see [Copy Data to Amazon Redshift Using AWS Data Pipeline](dp-copydata-redshift.md)\.

## Syntax<a name="redshiftcopyactivity-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| insertMode |   Determines what AWS Data Pipeline does with pre\-existing data in the target table that overlaps with rows in the data to be loaded\. Valid values are: `KEEP_EXISTING`, `OVERWRITE_EXISTING`, `TRUNCATE` and `APPEND`\. `KEEP_EXISTING` adds new rows to the table, while leaving any existing rows unmodified\. `KEEP_EXISTING` and ` OVERWRITE_EXISTING` use the primary key, sort, and distribution keys to identify which incoming rows to match with existing rows\. See [Updating and Inserting New Data](https://docs.aws.amazon.com/redshift/latest/dg/t_updating-inserting-using-staging-tables-.html) in the Amazon Redshift *Database Developer Guide*\.  `TRUNCATE` deletes all the data in the destination table before writing the new data\.  `APPEND` adds all records to the end of the Redshift table\. `APPEND` does not require a primary, distribution key, or sort key so items that may be potential duplicates may be appended\.  | Enumeration | 


****  

| Object Invocation Fields | Description | Slot Type | 
| --- | --- | --- | 
| schedule |  This object is invoked within the execution of a schedule interval\.  Specify a schedule reference to another object to set the dependency execution order for this object\.  In most cases, we recommend to put the schedule reference on the default pipeline object so that all objects inherit that schedule\. For example, you can explicitly set a schedule on the object by specifying `"schedule": {"ref": "DefaultSchedule"}`\.  If the master schedule in your pipeline contains nested schedules, create a parent object that has a schedule reference\.  For more information about example optional schedule configurations, see [Schedule](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-schedule.html)\.   | Reference Object, such as: "schedule":\{"ref":"myScheduleId"\} | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| runsOn | The computational resource to run the activity or command\. For example, an Amazon EC2 instance or Amazon EMR cluster\. | Reference Object, e\.g\. "runsOn":\{"ref":"myResourceId"\} | 
| workerGroup | The worker group\. This is used for routing tasks\. If you provide a runsOn value and workerGroup exists, workerGroup is ignored\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| attemptStatus | Most recently reported status from the remote activity\. | String | 
| attemptTimeout | Timeout for remote work completion\. If set, then a remote activity that does not complete within the set time of starting may be retried\. | Period | 
| commandOptions |  Takes parameters to pass to the Amazon Redshift data node during the `COPY` operation\. For information on parameters, see [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) in the Amazon Redshift *Database Developer Guide*\. As it loads the table, `COPY` attempts to implicitly convert the strings to the data type of the target column\. In addition to the default data conversions that happen automatically, if you receive errors or have other conversion needs, you can specify additional conversion parameters\. For information, see [Data Conversion Parameters](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-conversion.html) in the Amazon Redshift *Database Developer Guide*\. If a data format is associated with the input or output data node, then the provided parameters are ignored\.  Because the copy operation first uses `COPY` to insert data into a staging table, and then uses an `INSERT` command to copy the data from the staging table into the destination table, some `COPY` parameters do not apply, such as the `COPY` command's ability to enable automatic compression of the table\. If compression is required, add column encoding details to the `CREATE TABLE` statement\.  Also, in some cases when it needs to unload data from the Amazon Redshift cluster and create files in Amazon S3, the `RedshiftCopyActivity` relies on the `UNLOAD` operation from Amazon Redshift\. To improve performance during copying and unloading, specify `PARALLEL OFF` parameter from the `UNLOAD` command\. For information on parameters, see [UNLOAD](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) in the Amazon Redshift *Database Developer Guide*\.  | String | 
| dependsOn | Specify dependency on another runnable object\. | Reference Object: "dependsOn":\{"ref":"myActivityId"\} | 
| failureAndRerunMode | Describes consumer node behavior when dependencies fail or are rerun | Enumeration | 
| input | The input data node\. The data source can be Amazon S3, DynamoDB, or Amazon Redshift\. | Reference Object: "input":\{"ref":"myDataNodeId"\} | 
| lateAfterTimeout | The elapsed time after pipeline start within which the object must start\. It is triggered only when the schedule type is not set to ondemand\. | Period | 
| maxActiveInstances | The maximum number of concurrent active instances of a component\. Re\-runs do not count toward the number of active instances\. | Integer | 
| maximumRetries | Maximum number attempt retries on failure | Integer | 
| onFail | An action to run when current object fails\. | Reference Object: "onFail":\{"ref":"myActionId"\} | 
| onLateAction | Actions that should be triggered if an object has not yet been scheduled or still not completed\. | Reference Object:  "onLateAction":\{"ref":"myActionId"\} | 
| onSuccess | An action to run when current object succeeds\. | Reference Object: "onSuccess":\{"ref":"myActionId"\} | 
| output | The output data node\. The output location can be Amazon S3 or Amazon Redshift\. | Reference Object: "output":\{"ref":"myDataNodeId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object: "parent":\{"ref":"myBaseObjectId"\} | 
| pipelineLogUri | The S3 URI \(such as 's3://BucketName/Key/'\) for uploading logs for the pipeline\. | String | 
| precondition | Optionally define a precondition\. A data node is not marked "READY" until all preconditions have been met\. | Reference Object: "precondition":\{"ref":"myPreconditionId"\} | 
| queue |  Corresponds to the `query_group ` setting in Amazon Redshift, which allows you to assign and prioritize concurrent activities based on their placement in queues\.  Amazon Redshift limits the number of simultaneous connections to 15\. For more information, see [Assigning Queries to Queues](https://docs.aws.amazon.com/AmazonRDS/latest/DeveloperGuide/cm-c-executing-queries.html) in the Amazon RDS *Database Developer Guide*\.  | String | 
| reportProgressTimeout |  Timeout for remote work successive calls to `reportProgress`\.  If set, then remote activities that do not report progress for the specified period may be considered stalled and so retried\.  | Period | 
| retryDelay | The timeout duration between two retry attempts\. | Period | 
| scheduleType |  Allows you to specify whether the schedule for objects in your pipeline\. Values are: `cron`, `ondemand`, and `timeseries`\. The `timeseries` scheduling means instances are scheduled at the end of each interval\. The `Cron` scheduling means instances are scheduled at the beginning of each interval\.  An `ondemand` schedule allows you to run a pipeline one time per activation\. This means you do not have to clone or re\-create the pipeline to run it again\.  To use `ondemand` pipelines, call the `ActivatePipeline` operation for each subsequent run\.  If you use an `ondemand` schedule, you must specify it in the default object, and it must be the only `scheduleType` specified for objects in the pipeline\.  | Enumeration | 
| transformSql |  The `SQL SELECT` expression used to transform the input data\.  Run the `transformSql` expression on the table named `staging`\.  When you copy data from DynamoDB or Amazon S3, AWS Data Pipeline creates a table called "staging" and initially loads data in there\. Data from this table is used to update the target table\.  The output schema of `transformSql` must match the final target table's schema\. If you specify the `transformSql` option, a second staging table is created from the specified SQL statement\. The data from this second staging table is then updated in the final target table\.  | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @activeInstances | List of the currently scheduled active instance objects\. | Reference Object: "activeInstances":\{"ref":"myRunnableObjectId"\} | 
| @actualEndTime | Time when the execution of this object finished\. | DateTime | 
| @actualStartTime | Time when the execution of this object started\. | DateTime | 
| cancellationReason | The cancellationReason if this object was cancelled\. | String | 
| @cascadeFailedOn | Description of the dependency chain the object failed on\. | Reference Object: "cascadeFailedOn":\{"ref":"myRunnableObjectId"\} | 
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
| @scheduledEndTime | Schedule end time for object\. | DateTime | 
| @scheduledStartTime | Schedule start time for object\. | DateTime | 
| @status | The status of this object\. | String | 
| @version | Pipeline version the object was created with\. | String | 
| @waitingOn | Description of list of dependencies this object is waiting on\. | Reference Object: "waitingOn":\{"ref":"myRunnableObjectId"\} | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | Id of the pipeline to which this object belongs to\. | String | 
| @sphere | The sphere of an object\. Denotes its place in the life cycle\. For example, Component Objects give rise to Instance Objects which execute Attempt Objects\. | String | 