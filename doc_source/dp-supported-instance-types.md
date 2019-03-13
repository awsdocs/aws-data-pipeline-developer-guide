# Supported Instance Types for Pipeline Work Activities<a name="dp-supported-instance-types"></a>

When AWS Data Pipeline runs a pipeline, it compiles the pipeline components to create a set of actionable Amazon EC2 instances\. Each instance contains all the information for performing a specific task\. The complete set of instances is the to\-do list of the pipeline\. AWS Data Pipeline hands the instances out to task runners to process\.

EC2 instances come in different configurations, which are known as *instance types*\. Each instance type has a different CPU, input/output, and storage capacity\. In addition to specifying the instance type for an activity, you can choose different purchasing options\. Not all instance types are available in all AWS Regions\. If an instance type is not available, your pipeline may fail to provision or may be stuck provisioning\. For information about instance availability, see the [Amazon EC2 Pricing Page](https://aws.amazon.com//ec2/pricing)\. Open the link for your instance purchasing option and filter by **Region** to see if an instance type is available in the Region\. For more information about these instance types, families, and virtualization types, see [Amazon EC2 Instances](https://aws.amazon.com/ec2/instance-types/) and [Amazon Linux AMI Instance Type Matrix](https://aws.amazon.com/amazon-linux-ami/instance-type-matrix/)\.

The following tables describe the instance types that AWS Data Pipeline supports\. You can use AWS Data Pipeline to launch Amazon EC2 instances in any Region, including Regions where AWS Data Pipeline is not supported\. For information about Regions where AWS Data Pipeline is supported, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#datapipeline_region)\. 

**Topics**
+ [Default Amazon EC2 Instances by AWS Region](dp-ec2-default-instance-types.md)
+ [Additional Supported Amazon EC2 Instances](dp-ec2-supported-instance-types.md)
+ [Supported Amazon EC2 Instances for Amazon EMR Clusters](dp-emr-supported-instance-types.md)