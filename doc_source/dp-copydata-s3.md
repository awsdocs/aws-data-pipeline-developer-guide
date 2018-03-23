# Copy CSV Data Between Amazon S3 Buckets Using AWS Data Pipeline<a name="dp-copydata-s3"></a>

After you read [What is AWS Data Pipeline?](what-is-datapipeline.md) and decide you want to use AWS Data Pipeline to automate the movement and transformation of your data, it is time to get started with creating data pipelines\. To help you make sense of how AWS Data Pipeline works, let’s walk through a simple task\. 

This tutorial walks you through the process of creating a data pipeline to copy data from one Amazon S3 bucket to another and then send an Amazon SNS notification after the copy activity completes successfully\. You use an EC2 instance managed by AWS Data Pipeline for this copy activity\.

**Pipeline Objects**  
The pipeline uses the following objects:

[CopyActivity](dp-object-copyactivity.md)  
The activity that AWS Data Pipeline performs for this pipeline \(copy CSV data from one Amazon S3 bucket to another\)\.  
There are limitations when using the CSV file format with `CopyActivity` and `S3DataNode`\. For more information, see [CopyActivity](dp-object-copyactivity.md)\.

[Schedule](dp-object-schedule.md)  
The start date, time, and the recurrence for this activity\. You can optionally specify the end date and time\.

[Ec2Resource](dp-object-ec2resource.md)  
The resource \(an EC2 instance\) that AWS Data Pipeline uses to perform this activity\.

[S3DataNode](dp-object-s3datanode.md)  
The input and output nodes \(Amazon S3 buckets\) for this pipeline\.

[SnsAlarm](dp-object-snsalarm.md)  
The action AWS Data Pipeline must take when the specified conditions are met \(send Amazon SNS notifications to a topic after the task finishes successfully\)\. 

**Topics**
+ [Before You Begin](dp-copydata-s3-prereq.md)
+ [Copy CSV Data Using the AWS Data Pipeline Console](dp-copydata-s3-console.md)
+ [Copy CSV Data Using the Command Line](dp-get-started-copy-data-cli.md)