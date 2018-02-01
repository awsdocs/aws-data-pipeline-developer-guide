# Activities<a name="dp-concepts-activities"></a>

In AWS Data Pipeline, an activity is a pipeline component that defines the work to perform\. AWS Data Pipeline provides several pre\-packaged activities that accommodate common scenarios, such as moving data from one location to another, running Hive queries, and so on\. Activities are extensible, so you can run your own custom scripts to support endless combinations\.

AWS Data Pipeline supports the following types of activities:

[CopyActivity](dp-object-copyactivity.md)  
Copies data from one location to another\.

[EmrActivity](dp-object-emractivity.md)  
Runs an Amazon EMR cluster\.

[HiveActivity](dp-object-hiveactivity.md)  
Runs a Hive query on an Amazon EMR cluster\.

[HiveCopyActivity](dp-object-hivecopyactivity.md)  
Runs a Hive query on an Amazon EMR cluster with support for advanced data filtering and support for [S3DataNode](dp-object-s3datanode.md) and [DynamoDBDataNode](dp-object-dynamodbdatanode.md)\.

[PigActivity](dp-object-pigactivity.md)  
Runs a Pig script on an Amazon EMR cluster\.

[RedshiftCopyActivity](dp-object-redshiftcopyactivity.md)  
Copies data to and from Amazon Redshift tables\.

[ShellCommandActivity](dp-object-shellcommandactivity.md)  
Runs a custom UNIX/Linux shell command as an activity\.

[SqlActivity](dp-object-sqlactivity.md)  
Runs a SQL query on a database\.

Some activities have special support for staging data and database tables\. For more information, see [Staging Data and Tables with Pipeline Activities](dp-concepts-staging.md)\.