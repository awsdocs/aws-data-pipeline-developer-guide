# Data Nodes<a name="dp-concepts-datanodes"></a>

In AWS Data Pipeline, a data node defines the location and type of data that a pipeline activity uses as input or output\. AWS Data Pipeline supports the following types of data nodes:

[DynamoDBDataNode](dp-object-dynamodbdatanode.md)  
A DynamoDB table that contains data for [HiveActivity](dp-object-hiveactivity.md) or [EmrActivity](dp-object-emractivity.md) to use\.

[SqlDataNode](dp-object-sqldatanode.md)  
An SQL table and database query that represent data for a pipeline activity to use\.  
Previously, MySqlDataNode was used\. Use SqlDataNode instead\.

[RedshiftDataNode](dp-object-redshiftdatanode.md)  
An Amazon Redshift table that contains data for [RedshiftCopyActivity](dp-object-redshiftcopyactivity.md) to use\.

[S3DataNode](dp-object-s3datanode.md)  
An Amazon S3 location that contains one or more files for a pipeline activity to use\.