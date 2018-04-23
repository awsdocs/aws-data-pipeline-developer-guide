# DynamoDBDataFormat<a name="dp-object-dynamodbdataformat"></a>

Applies a schema to a DynamoDB table to make it accessible by a Hive query\. `DynamoDBDataFormat` is used with a `HiveActivity` object and a `DynamoDBDataNode` input and output\. `DynamoDBDataFormat` requires that you specify all columns in your Hive query\. For more flexibility to specify certain columns in a Hive query or Amazon S3 support, see [DynamoDBExportDataFormat](dp-object-dynamodbexportdataformat.md)\.

**Note**  
DynamoDB Boolean types are not mapped to Hive Boolean types\. However, it is possible to map DynamoDB integer values of 0 or 1 to Hive Boolean types\.

## Example<a name="dynamodbdataformat-example"></a>

The following example shows how to use `DynamoDBDataFormat` to assign a schema to a `DynamoDBDataNode` input, which allows a `HiveActivity` object to access the data by named columns and copy the data to a `DynamoDBDataNode` output\. 

```
{
  "objects": [
    {
      "id" : "Exists.1",
      "name" : "Exists.1",
      "type" : "Exists"
    },
    {
      "id" : "DataFormat.1",
      "name" : "DataFormat.1",
      "type" : "DynamoDBDataFormat",
      "column" : [ 
         "hash STRING", 
        "range STRING" 
      ]
    },
    {
      "id" : "DynamoDBDataNode.1",
      "name" : "DynamoDBDataNode.1",
      "type" : "DynamoDBDataNode",
      "tableName" : "$INPUT_TABLE_NAME",
      "schedule" : { "ref" : "ResourcePeriod" },
      "dataFormat" : { "ref" : "DataFormat.1" }
    },
    {
      "id" : "DynamoDBDataNode.2",
      "name" : "DynamoDBDataNode.2",
      "type" : "DynamoDBDataNode",
      "tableName" : "$OUTPUT_TABLE_NAME",
      "schedule" : { "ref" : "ResourcePeriod" },
      "dataFormat" : { "ref" : "DataFormat.1" }
    },
    {
      "id" : "EmrCluster.1",
      "name" : "EmrCluster.1",
      "type" : "EmrCluster",
      "schedule" : { "ref" : "ResourcePeriod" },
      "masterInstanceType" : "m1.small",
      "keyPair" : "$KEYPAIR"
    },
    {
      "id" : "HiveActivity.1",
      "name" : "HiveActivity.1",
      "type" : "HiveActivity",
      "input" : { "ref" : "DynamoDBDataNode.1" },
      "output" : { "ref" : "DynamoDBDataNode.2" },
      "schedule" : { "ref" : "ResourcePeriod" },
      "runsOn" : { "ref" : "EmrCluster.1" },
      "hiveScript" : "insert overwrite table ${output1} select * from ${input1} ;"
    },
    {
      "id" : "ResourcePeriod",
      "name" : "ResourcePeriod",
      "type" : "Schedule",
      "period" : "1 day",
      "startDateTime" : "2012-05-04T00:00:00",
      "endDateTime" : "2012-05-05T00:00:00"
    }
  ]
}
```

## Syntax<a name="dynamodbdataformat-syntax"></a>


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| column | The column name with data type specified by each field for the data described by this data node\. For example, hostname STRING\. For multiple values, use column names and data types separated by a space\. | String | 
| parent | The parent of the current object from which slots will be inherited\. | Reference Object, such as "parent":\{"ref":"myBaseObjectId"\} | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @version | The pipeline version uses to create the object\. | String | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | The error describing the ill\-formed object\. | String | 
| @pipelineId | The Id of the pipeline to which this object belongs\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects\. | String | 