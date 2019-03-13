# Before You Begin<a name="dp-copydata-mysql-prereq"></a>

Be sure you've completed the following steps\. 
+ Complete the tasks in [Setting up for AWS Data Pipeline](dp-get-setup.md)\.
+ \(Optional\) Set up a VPC for the instance and a security group for the VPC\. For more information, see [Launching Resources for Your Pipeline into a VPC](dp-resources-vpc.md)\.
+ Create an Amazon S3 bucket as a data output\.

  For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in *Amazon Simple Storage Service Getting Started Guide*\.
+ Create and launch a MySQL database instance as your data source\. 

  For more information, see [Launch a DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/GettingStartedGuide/LaunchDBInstance.html) in the *Amazon RDS Getting Started Guide*\. After you have an Amazon RDS instance, see [Create a Table](http://dev.mysql.com/doc/refman/5.5/en//creating-tables.html) in the MySQL documentation\.
**Note**  
Make a note of the user name and the password you used for creating the MySQL instance\. After you've launched your MySQL database instance, make a note of the instance's endpoint\. You'll need this information later\.
+ Connect to your MySQL database instance, create a table, and then add test data values to the newly created table\.

  For illustration purposes, we created this tutorial using a MySQL table with the following configuration and sample data\. The following screen shot is from MySQL Workbench 5\.2 CE:   
![\[Sample MySQL table configuration\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-tutorial-rdstos3-sampletable.png)

  For more information, see [Create a Table](http://dev.mysql.com/doc/refman/5.5/en//creating-tables.html) in the MySQL documentation and the [MySQL Workbench product page](http://www.mysql.com/products/workbench/)\.
+ Create a topic for sending email notification and make a note of the topic Amazon Resource Name \(ARN\)\. For more information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/gsg/CreateTopic.html) in *Amazon Simple Notification Service Getting Started Guide*\.
+ \(Optional\) This tutorial uses the default IAM role policies created by AWS Data Pipeline\. If you would rather create and configure your IAM role policy and trust relationships, follow the instructions described in [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)\. 