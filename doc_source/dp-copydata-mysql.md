# Export MySQL Data to Amazon S3 Using AWS Data Pipeline<a name="dp-copydata-mysql"></a>

This tutorial walks you through the process of creating a data pipeline to copy data \(rows\) from a table in MySQL database to a CSV \(comma\-separated values\) file in an Amazon S3 bucket and then sending an Amazon SNS notification after the copy activity completes successfully\. You will use an EC2 instance provided by AWS Data Pipeline for this copy activity\.

**Pipeline Objects**  
The pipeline uses the following objects:
+ [CopyActivity](dp-object-copyactivity.md)
+ [Ec2Resource](dp-object-ec2resource.md)
+ [MySqlDataNode](dp-object-mysqldatanode.md)
+ [S3DataNode](dp-object-s3datanode.md)
+ [SnsAlarm](dp-object-snsalarm.md)

**Topics**
+ [Before You Begin](dp-copydata-mysql-prereq.md)
+ [Copy MySQL Data Using the AWS Data Pipeline Console](dp-copydata-mysql-console.md)
+ [Copy MySQL Data Using the Command Line](dp-copymysql-cli.md)