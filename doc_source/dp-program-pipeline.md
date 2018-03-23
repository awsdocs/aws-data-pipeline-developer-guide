# Working with the API<a name="dp-program-pipeline"></a>

**Note**  
 If you are not writing programs that interact with AWS Data Pipeline, you do not need to install any of the AWS SDKs\. You can create and run pipelines using the console or command\-line interface\. For more information, see [Setting up for AWS Data Pipeline](dp-get-setup.md)

 The easiest way to write applications that interact with AWS Data Pipeline or to implement a custom Task Runner is to use one of the AWS SDKs\. The AWS SDKs provide functionality that simplifies calling the web service APIs from your preferred programming environment\. For more information, see [Install the AWS SDK](#dp-set-up-aws-sdk)\. 

## Install the AWS SDK<a name="dp-set-up-aws-sdk"></a>

 The AWS SDKs provide functions that wrap the API and take care of many of the connection details, such as calculating signatures, handling request retries, and error handling\. The SDKs also contain sample code, tutorials, and other resources to help you get started writing applications that call AWS\. Calling the wrapper functions in an SDK can greatly simplify the process of writing an AWS application\. For more information about how to download and use the AWS SDKs, go to [Sample Code & Libraries](http://aws.amazon.com/code)\. 

AWS Data Pipeline support is available in SDKs for the following platforms: 
+  [AWS SDK for Java](http://aws.amazon.com/java) 
+  [AWS SDK for Node\.js](http://aws.amazon.com/sdkfornodejs) 
+  [AWS SDK for PHP](http://aws.amazon.com/sdkforphp) 
+  [AWS SDK for Python \(Boto\)](http://aws.amazon.com/sdkforpython) 
+  [AWS SDK for Ruby](http://aws.amazon.com/sdkforruby) 
+  [AWS SDK for \.NET](http://aws.amazon.com/net) 