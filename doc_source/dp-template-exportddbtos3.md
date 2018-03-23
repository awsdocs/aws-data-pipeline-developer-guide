# Export DynamoDB Table to S3<a name="dp-template-exportddbtos3"></a>

The **Export DynamoDB table to S3** template schedules an Amazon EMR cluster to export data from a DynamoDB table to an Amazon S3 bucket\. This template uses an Amazon EMR cluster, which is sized proportionally to the value of the throughput available to the DynamoDB table\. Although you can increase IOPs on a table, this may incur additional costs while importing and exporting\. Previously, export used a HiveActivity but now uses native MapReduce\.

The template uses the following pipeline objects:
+ [EmrActivity](dp-object-emractivity.md)
+ [EmrCluster](dp-object-emrcluster.md)
+ [DynamoDBDataNode](dp-object-dynamodbdatanode.md)
+ [S3DataNode](dp-object-s3datanode.md)

For a tutorial, see [Import and Export DynamoDB Data Using AWS Data Pipeline](dp-importexport-ddb.md)\.