# Copy MySQL Data Using the Command Line<a name="dp-copymysql-cli"></a>

You can create a pipeline to copy data from a MySQL table to a file in an Amazon S3 bucket\.

**Prerequisites**

Before you begin, you must complete the following steps:

1. Install and configure a command line interface \(CLI\)\. For more information, see [Accessing AWS Data Pipeline](what-is-datapipeline.md#accessing-datapipeline)\.

1. Ensure that the IAM roles named **DataPipelineDefaultRole** and **DataPipelineDefaultResourceRole** exist\. The AWS Data Pipeline console creates these roles for you automatically\. If you haven't used the AWS Data Pipeline console at least once, then you must create these roles manually\. For more information, see [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)\.

1. Set up an Amazon S3 bucket and an Amazon RDS instance\. For more information, see [Before You Begin](dp-copydata-mysql-prereq.md)\.

**Topics**
+ [Define a Pipeline in JSON Format](#dp-copymysql-define-pipeline-cli)
+ [Upload and Activate the Pipeline Definition](#dp-copymysql-json-upload-cli)

## Define a Pipeline in JSON Format<a name="dp-copymysql-define-pipeline-cli"></a>

This example scenario shows how to use JSON pipeline definitions and the AWS Data Pipeline CLI to copy data \(rows\) from a table in a MySQL database to a CSV \(comma\-separated values\) file in an Amazon S3 bucket at a specified time interval\.

This is the full pipeline definition JSON file followed by an explanation for each of its sections\. 

**Note**  
 We recommend that you use a text editor that can help you verify the syntax of JSON\-formatted files, and name the file using the \.json file extension\. 

```
{
  "objects": [
    {
      "id": "ScheduleId113",
      "startDateTime": "2013-08-26T00:00:00",
      "name": "My Copy Schedule",
      "type": "Schedule",
      "period": "1 Days"
    },
    {
      "id": "CopyActivityId112",
      "input": {
        "ref": "MySqlDataNodeId115"
      },
      "schedule": {
        "ref": "ScheduleId113"
      },
      "name": "My Copy",
      "runsOn": {
        "ref": "Ec2ResourceId116"
      },
      "onSuccess": {
        "ref": "ActionId1"
      },
      "onFail": {
        "ref": "SnsAlarmId117"
      },
      "output": {
        "ref": "S3DataNodeId114"
      },
      "type": "CopyActivity"
    },
    {
      "id": "S3DataNodeId114",
      "schedule": {
        "ref": "ScheduleId113"
      },
      "filePath": "s3://example-bucket/rds-output/output.csv",
      "name": "My S3 Data",
      "type": "S3DataNode"
    },
    {
      "id": "MySqlDataNodeId115",
      "username": "my-username",
      "schedule": {
        "ref": "ScheduleId113"
      },
      "name": "My RDS Data",
      "*password": "my-password",
      "table": "table-name",
      "connectionString": "jdbc:mysql://your-sql-instance-name.id.region-name.rds.amazonaws.com:3306/database-name",
      "selectQuery": "select * from #{table}",
      "type": "SqlDataNode"
    },
    {
      "id": "Ec2ResourceId116",
      "schedule": {
        "ref": "ScheduleId113"
      },
      "name": "My EC2 Resource",
      "role": "DataPipelineDefaultRole",
      "type": "Ec2Resource",
      "resourceRole": "DataPipelineDefaultResourceRole"
    },
    {
      "message": "This is a success message.",
      "id": "ActionId1",
      "subject": "RDS to S3 copy succeeded!",
      "name": "My Success Alarm",
      "role": "DataPipelineDefaultRole",
      "topicArn": "arn:aws:sns:us-east-1:123456789012:example-topic",
      "type": "SnsAlarm"
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
      "message": "There was a problem executing #{node.name} at for period #{node.@scheduledStartTime} to #{node.@scheduledEndTime}",
      "id": "SnsAlarmId117",
      "subject": "RDS to S3 copy failed",
      "name": "My Failure Alarm",
      "role": "DataPipelineDefaultRole",
      "topicArn": "arn:aws:sns:us-east-1:123456789012:example-topic",
      "type": "SnsAlarm"
    }
  ]
}
```

### MySQL Data Node<a name="dp-copymysql-rds-node-cli"></a>

The input MySqlDataNode pipeline component defines a location for the input data; in this case, an Amazon RDS instance\. The input MySqlDataNode component is defined by the following fields: 

```
{
  "id": "MySqlDataNodeId115",
  "username": "my-username",
  "schedule": {
    "ref": "ScheduleId113"
  },
  "name": "My RDS Data",
  "*password": "my-password",
  "table": "table-name",
  "connectionString": "jdbc:mysql://your-sql-instance-name.id.region-name.rds.amazonaws.com:3306/database-name",
  "selectQuery": "select * from #{table}",
  "type": "SqlDataNode"
},
```

Id  
The user\-defined name, which is a label for your reference only\.

Username  
The user name of the database account that has sufficient permission to retrieve data from the database table\. Replace *my\-username* with the name of your user account\.

Schedule  
A reference to the schedule component that we created in the preceding lines of the JSON file\.

Name  
The user\-defined name, which is a label for your reference only\.

\*Password  
The password for the database account with the asterisk prefix to indicate that AWS Data Pipeline must encrypt the password value\. Replace *my\-password* with the correct password for your user account\. The password field is preceded by the asterisk special character\. For more information, see [Special Characters](dp-pipeline-characters.md)\.

Table  
The name of the database table that contains the data to copy\. Replace *table\-name* with the name of your database table\.

connectionString  
The JDBC connection string for the CopyActivity object to connect to the database\.

selectQuery  
A valid SQL SELECT query that specifies which data to copy from the database table\. Note that `#{table}` is an expression that re\-uses the table name provided by the "table" variable in the preceding lines of the JSON file\.

Type  
The SqlDataNode type, which is an Amazon RDS instance using MySQL in this example\.  
The MySqlDataNode type is deprecated\. While you can still use MySqlDataNode, we recommend using SqlDataNode\. 

### Amazon S3 Data Node<a name="dp-copymysql-json-s3-node-cli"></a>

Next, the S3Output pipeline component defines a location for the output file; in this case a CSV file in an Amazon S3 bucket location\. The output S3DataNode component is defined by the following fields: 

```
{
  "id": "S3DataNodeId114",
  "schedule": {
    "ref": "ScheduleId113"
  },
  "filePath": "s3://example-bucket/rds-output/output.csv",
  "name": "My S3 Data",
  "type": "S3DataNode"
},
```

Id  
The user\-defined ID, which is a label for your reference only\.

Schedule  
A reference to the schedule component that we created in the preceding lines of the JSON file\.

filePath  
The path to the data associated with the data node, which is an CSV output file in this example\. 

Name  
The user\-defined name, which is a label for your reference only\.

Type  
The pipeline object type, which is S3DataNode to match the location where the data resides, in an Amazon S3 bucket\.

### Resource<a name="dp-copymysql-json-resource-cli"></a>

This is a definition of the computational resource that performs the copy operation\. In this example, AWS Data Pipeline should automatically create an EC2 instance to perform the copy task and terminate the resource after the task completes\. The fields defined here control the creation and function of the EC2 instance that does the work\. The EC2Resource is defined by the following fields: 

```
{
  "id": "Ec2ResourceId116",
  "schedule": {
    "ref": "ScheduleId113"
  },
  "name": "My EC2 Resource",
  "role": "DataPipelineDefaultRole",
  "type": "Ec2Resource",
  "resourceRole": "DataPipelineDefaultResourceRole"
},
```

Id  
The user\-defined ID, which is a label for your reference only\.

Schedule  
The schedule on which to create this computational resource\.

Name  
The user\-defined name, which is a label for your reference only\.

Role  
The IAM role of the account that accesses resources, such as accessing an Amazon S3 bucket to retrieve data\.

Type  
The type of computational resource to perform work; in this case, an EC2 instance\. There are other resource types available, such as an EmrCluster type\.

resourceRole  
The IAM role of the account that creates resources, such as creating and configuring an EC2 instance on your behalf\. Role and ResourceRole can be the same role, but separately provide greater granularity in your security configuration\.

### Activity<a name="dp-copymysql-json-activity-cli"></a>

The last section in the JSON file is the definition of the activity that represents the work to perform\. In this case we use a CopyActivity component to copy data from a file in an Amazon S3 bucket to another file\. The CopyActivity component is defined by the following fields: 

```
{
  "id": "CopyActivityId112",
  "input": {
    "ref": "MySqlDataNodeId115"
  },
  "schedule": {
    "ref": "ScheduleId113"
  },
  "name": "My Copy",
  "runsOn": {
    "ref": "Ec2ResourceId116"
  },
  "onSuccess": {
    "ref": "ActionId1"
  },
  "onFail": {
    "ref": "SnsAlarmId117"
  },
  "output": {
    "ref": "S3DataNodeId114"
  },
  "type": "CopyActivity"
},
```

Id  
The user\-defined ID, which is a label for your reference only

Input  
The location of the MySQL data to copy

Schedule  
The schedule on which to run this activity

Name  
The user\-defined name, which is a label for your reference only

runsOn  
The computational resource that performs the work that this activity defines\. In this example, we provide a reference to the EC2 instance defined previously\. Using the `runsOn` field causes AWS Data Pipeline to create the EC2 instance for you\. The `runsOn` field indicates that the resource exists in the AWS infrastructure, while the workerGroup value indicates that you want to use your own on\-premises resources to perform the work\.

onSuccess  
The [SnsAlarm](dp-object-snsalarm.md) to send if the activity completes successfully

onFail  
The [SnsAlarm](dp-object-snsalarm.md) to send if the activity fails

Output  
The Amazon S3 location of the CSV output file

Type  
The type of activity to perform\.

## Upload and Activate the Pipeline Definition<a name="dp-copymysql-json-upload-cli"></a>

You must upload your pipeline definition and activate your pipeline\. In the following example commands, replace *pipeline\_name* with a label for your pipeline and *pipeline\_file* with the fully\-qualified path for the pipeline definition `.json` file\.

**AWS CLI**

To create your pipeline definition and activate your pipeline, use the following [create\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/create-pipeline.html) command\. Note the ID of your pipeline, because you'll use this value with most CLI commands\.

```
aws datapipeline create-pipeline --name pipeline_name --unique-id token
{
    "pipelineId": "df-00627471SOVYZEXAMPLE"
}
```

To upload your pipeline definition, use the following [put\-pipeline\-definition](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/put-pipeline-definition.html) command\.

```
aws datapipeline put-pipeline-definition --pipeline-id df-00627471SOVYZEXAMPLE --pipeline-definition file://MyEmrPipelineDefinition.json
```

If you pipeline validates successfully, the `validationErrors` field is empty\. You should review any warnings\.

To activate your pipeline, use the following [activate\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/activate-pipeline.html) command\.

```
aws datapipeline activate-pipeline --pipeline-id df-00627471SOVYZEXAMPLE
```

You can verify that your pipeline appears in the pipeline list using the following [list\-pipelines](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/list-pipelines.html) command\.

```
aws datapipeline list-pipelines
```