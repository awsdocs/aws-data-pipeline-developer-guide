# Before You Begin<a name="dp-copydata-s3-prereq"></a>

Be sure you've completed the following steps\. 
+ Complete the tasks in [Setting up for AWS Data Pipeline](dp-get-setup.md)\.
+ \(Optional\) Set up a VPC for the instance and a security group for the VPC\. For more information, see [Launching Resources for Your Pipeline into a VPC](dp-resources-vpc.md)\.
+ Create an Amazon S3 bucket as a data source\. 

  For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\.
+ Upload your data to your Amazon S3 bucket\. 

  For more information, see [Add an Object to a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\.
+ Create another Amazon S3 bucket as a data target
+ Create a topic for sending email notification and make a note of the topic Amazon Resource Name \(ARN\)\. For more information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/gsg/CreateTopic.html) in the *Amazon Simple Notification Service Getting Started Guide*\.
+ \(Optional\) This tutorial uses the default IAM role policies created by AWS Data Pipeline\. If you would rather create and configure your own IAM role policy and trust relationships, follow the instructions described in [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)\. 