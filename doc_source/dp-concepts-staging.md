# Staging Data and Tables with Pipeline Activities<a name="dp-concepts-staging"></a>

AWS Data Pipeline can stage input and output data in your pipelines to make it easier to use certain activities, such as `ShellCommandActivity` and `HiveActivity`\. 

Data staging enables you to copy data from the input data node to the resource executing the activity, and, similarly, from the resource to the output data node\. 

The staged data on the Amazon EMR or Amazon EC2 resource is available by using special variables in the activity's shell commands or Hive scripts\. 

Table staging is similar to data staging, except the staged data takes the form of database tables, specifically\. 

AWS Data Pipeline supports the following staging scenarios:
+ Data staging with `ShellCommandActivity`
+ Table staging with Hive and staging\-supported data nodes
+ Table staging with Hive and staging\-unsupported data nodes

**Note**  
Staging only functions when the `stage` field is set to `true` on an activity, such as `ShellCommandActivity`\. For more information, see [ShellCommandActivity](dp-object-shellcommandactivity.md)\.

In addition, data nodes and activities can relate in four ways:

Staging data locally on a resource  
The input data automatically copies into the resource local file system\. Output data automatically copies from the resource local file system to the output data node\. For example, when you configure `ShellCommandActivity` inputs and outputs with staging = true, the input data is available as INPUT*x*\_STAGING\_DIR and output data is available as OUTPUT*x*\_STAGING\_DIR, where *x* is the number of input or output\.

Staging input and output definitions for an activity  
The input data format \(column names and table names\) automatically copies into the activity's resource\. For example, when you configure `HiveActivity` with staging = true\. The data format specified on the input `S3DataNode` is used to stage the table definition from the Hive table\.

Staging not enabled  
The input and output objects and their fields are available for the activity, but the data itself is not\. For example, `EmrActivity` by default or when you configure other activities with staging = false\. In this configuration, the data fields are available for the activity to make a reference to them using the AWS Data Pipeline expression syntax, and this only occurs when the dependency is satisfied\. This serves as dependency checking only\. Code in the activity is responsible for copying the data from the input to the resource running the activity\.

Dependency relationship between objects  
There is a depends\-on relationship between two objects, which results in a similar situation to when staging is not enabled\. This causes a data node or activity to act as a precondition for the execution of another activity\.

## Data Staging with ShellCommandActivity<a name="dp-concepts-datastaging"></a>

Consider a scenario using a `ShellCommandActivity` with `S3DataNode` objects as data input and output\. AWS Data Pipeline automatically stages the data nodes to make them accessible to the shell command as if they were local file folders using the environment variables `${INPUT1_STAGING_DIR}` and `${OUTPUT1_STAGING_DIR}` as shown in the following example\. The numeric portion of the variables named `INPUT1_STAGING_DIR` and `OUTPUT1_STAGING_DIR` increment depending on the number of data nodes your activity references\.

**Note**  
 This scenario only works as described if your data inputs and outputs are `S3DataNode` objects\. Additionally, output data staging is allowed only when `directoryPath` is set on the output `S3DataNode` object\. 

```
{
  "id": "AggregateFiles",
  "type": "ShellCommandActivity",
  "stage": "true",
  "command": "cat ${INPUT1_STAGING_DIR}/part* > ${OUTPUT1_STAGING_DIR}/aggregated.csv",
  "input": {
    "ref": "MyInputData"
  },
  "output": {
    "ref": "MyOutputData"
  }
},
{
  "id": "MyInputData",
  "type": "S3DataNode",
  "schedule": {
    "ref": "MySchedule"
  },
  "filePath": "s3://my_bucket/source/#{format(@scheduledStartTime,'YYYY-MM-dd_HHmmss')}/items"
  }
},                    
{
  "id": "MyOutputData",
  "type": "S3DataNode",
  "schedule": {
    "ref": "MySchedule"
  },
  "directoryPath": "s3://my_bucket/destination/#{format(@scheduledStartTime,'YYYY-MM-dd_HHmmss')}"
  }
},
...
```

## Table Staging with Hive and Staging\-Supported Data Nodes<a name="dp-concepts-tablestaging"></a>

Consider a scenario using a `HiveActivity` with `S3DataNode` objects as data input and output\. AWS Data Pipeline automatically stages the data nodes to make them accessible to the Hive script as if they were Hive tables using the variables `${input1}` and `${output1}` as shown in the following example for `HiveActivity`\. The numeric portion of the variables named `input` and `output` increment depending on the number of data nodes your activity references\.

**Note**  
 This scenario only works as described if your data inputs and outputs are `S3DataNode` or `MySqlDataNode` objects\. Table staging is not supported for `DynamoDBDataNode`\.

```
{
  "id": "MyHiveActivity",
  "type": "HiveActivity",
  "schedule": {
    "ref": "MySchedule"
  },
  "runsOn": {
    "ref": "MyEmrResource"
  },
  "input": {
    "ref": "MyInputData"
  },
  "output": {
    "ref": "MyOutputData"
  },
  "hiveScript": "INSERT OVERWRITE TABLE ${output1} select * from ${input1};"
},
{
  "id": "MyInputData",
  "type": "S3DataNode",
  "schedule": {
    "ref": "MySchedule"
  },
  "directoryPath": "s3://test-hive/input"
  }
},                    
{
  "id": "MyOutputData",
  "type": "S3DataNode",
  "schedule": {
    "ref": "MySchedule"
  },
  "directoryPath": "s3://test-hive/output"
  }
},
...
```

## Table Staging with Hive and Staging\-Unsupported Data Nodes<a name="dp-concepts-nostaging"></a>

Consider a scenario using a `HiveActivity` with `DynamoDBDataNode` as data input and an `S3DataNode` object as the output\. No data staging is available for `DynamoDBDataNode`, therefore you must first manually create the table within your hive script, using the variable name `#{input.tableName}` to refer to the DynamoDB table\. Similar nomenclature applies if the DynamoDB table is the output, except you use variable `#{output.tableName}`\. Staging is available for the output `S3DataNode` object in this example, therefore you can refer to the output data node as `${output1}`\.

**Note**  
In this example, the table name variable has the \# \(hash\) character prefix because AWS Data Pipeline uses expressions to access the `tableName` or `directoryPath`\. For more information about how expression evaluation works in AWS Data Pipeline, see [Expression Evaluation](dp-pipeline-expressions.md#dp-datatype-functions)\.

```
{
  "id": "MyHiveActivity",
  "type": "HiveActivity",
  "schedule": {
    "ref": "MySchedule"
  },
  "runsOn": {
    "ref": "MyEmrResource"
  },
  "input": {
    "ref": "MyDynamoData"
  },
  "output": {
    "ref": "MyS3Data"
  },
  "hiveScript": "-- Map DynamoDB Table
SET dynamodb.endpoint=dynamodb.us-east-1.amazonaws.com;
SET dynamodb.throughput.read.percent = 0.5;
CREATE EXTERNAL TABLE dynamodb_table (item map<string,string>)
STORED BY 'org.apache.hadoop.hive.dynamodb.DynamoDBStorageHandler'
TBLPROPERTIES ("dynamodb.table.name" = "#{input.tableName}"); 
INSERT OVERWRITE TABLE ${output1} SELECT * FROM dynamodb_table;"
},
{
  "id": "MyDynamoData",
  "type": "DynamoDBDataNode",
  "schedule": {
    "ref": "MySchedule"
  },
  "tableName": "MyDDBTable"
},                 
{
  "id": "MyS3Data",
  "type": "S3DataNode",
  "schedule": {
    "ref": "MySchedule"
  },
  "directoryPath": "s3://test-hive/output"
  }
},
...
```