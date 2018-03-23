# Process Data Using Amazon EMR with Hadoop Streaming<a name="dp-launch-emr-jobflow"></a>

You can use AWS Data Pipeline to manage your Amazon EMR clusters\. With AWS Data Pipeline you can specify preconditions that must be met before the cluster is launched \(for example, ensuring that today's data been uploaded to Amazon S3\), a schedule for repeatedly running the cluster, and the cluster configuration to use\. The following tutorial walks you through launching a simple cluster\.

In this tutorial, you create a pipeline for a simple Amazon EMR cluster to run a pre\-existing Hadoop Streaming job provided by Amazon EMR and send an Amazon SNS notification after the task completes successfully\. You use the Amazon EMR cluster resource provided by AWS Data Pipeline for this task\. The sample application is called WordCount, and can also be run manually from the Amazon EMR console\. Note that clusters spawned by AWS Data Pipeline on your behalf are displayed in the Amazon EMR console and are billed to your AWS account\.

**Pipeline Objects**  
The pipeline uses the following objects:

[EmrActivity](dp-object-emractivity.md)  
Defines the work to perform in the pipeline \(run a pre\-existing Hadoop Streaming job provided by Amazon EMR\)\.

[EmrCluster](dp-object-emrcluster.md)  
Resource AWS Data Pipeline uses to perform this activity\.  
A cluster is a set of Amazon EC2 instances\. AWS Data Pipeline launches the cluster and then terminates it after the task finishes\. 

[Schedule](dp-object-schedule.md)  
Start date, time, and the duration for this activity\. You can optionally specify the end date and time\.

[SnsAlarm](dp-object-snsalarm.md)  
Sends an Amazon SNS notification to the topic you specify after the task finishes successfully\. 

**Topics**
+ [Before You Begin](dp-emr-jobflow-prereq.md)
+ [Launch a Cluster Using the AWS Data Pipeline Console](dp-emr-jobflow-console.md)
+ [Launch a Cluster Using the Command Line](dp-launch-emr-jobflow-cli.md)