# Data Nodes<a name="dp-copydata-redshift-node-cli"></a>

This example uses an input data node, an output data node, and a database\.

**Input Data Node**  
The input `S3DataNode` pipeline component defines the location of the input data in Amazon S3 and the data format of the input data\. For more information, see [S3DataNode](dp-object-s3datanode.md)\.

This input component is defined by the following fields:

```
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
```

`id`  
The user\-defined ID, which is a label for your reference only\.

`schedule`  
A reference to the schedule component\.

`filePath`  
The path to the data associated with the data node, which is an CSV input file in this example\.

`name`  
The user\-defined name, which is a label for your reference only\.

`dataFormat`  
A reference to the format of the data for the activity to process\.

**Output Data Node**  
The output `RedshiftDataNode` pipeline component defines a location for the output data; in this case, a table in an Amazon Redshift database\. For more information, see [RedshiftDataNode](dp-object-redshiftdatanode.md)\. This output component is defined by the following fields: 

```
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
```

`id`  
The user\-defined ID, which is a label for your reference only\.

`schedule`  
A reference to the schedule component\.

`tableName`  
The name of the Amazon Redshift table\.

`name`  
The user\-defined name, which is a label for your reference only\.

`createTableSql`  
A SQL expression to create the table in the database\.

`database`  
A reference to the Amazon Redshift database\.

**Database**  
The `RedshiftDatabase` component is defined by the following fields\. For more information, see [RedshiftDatabase](dp-object-redshiftdatabase.md)\.

```
{
  "id": "RedshiftDatabaseId1",
  "databaseName": "dbname",
  "username": "user",
  "name": "DefaultRedshiftDatabase1",
  "*password": "password",
  "type": "RedshiftDatabase",
  "clusterId": "redshiftclusterId"
},
```

`id`  
The user\-defined ID, which is a label for your reference only\.

`databaseName`  
The name of the logical database\.

`username`  
The user name to connect to the database\.

`name`  
The user\-defined name, which is a label for your reference only\.

`password`  
The password to connect to the database\.

`clusterId`  
The ID of the Redshift cluster\. 