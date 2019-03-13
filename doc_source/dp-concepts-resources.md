# Resources<a name="dp-concepts-resources"></a>

In AWS Data Pipeline, a resource is the computational resource that performs the work that a pipeline activity specifies\. AWS Data Pipeline supports the following types of resources:

[Ec2Resource](dp-object-ec2resource.md)  
An EC2 instance that performs the work defined by a pipeline activity\.

[EmrCluster](dp-object-emrcluster.md)  
An Amazon EMR cluster that performs the work defined by a pipeline activity, such as [EmrActivity](dp-object-emractivity.md)\.

Resources can run in the same region with their working dataset, even a region different than AWS Data Pipeline\. For more information, see [Using a Pipeline with Resources in Multiple Regions](dp-manage-region.md)\. 

## Resource Limits<a name="dp-resource-limits"></a>

 AWS Data Pipeline scales to accommodate a huge number of concurrent tasks and you can configure it to automatically create the resources necessary to handle large workloads\. These automatically created resources are under your control and count against your AWS account resource limits\. For example, if you configure AWS Data Pipeline to create a 20\-node Amazon EMR cluster automatically to process data and your AWS account has an EC2 instance limit set to 20, you may inadvertently exhaust your available backfill resources\. As a result, consider these resource restrictions in your design or increase your account limits accordingly\. For more information about service limits, see [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.

**Note**  
The limit is one instance per `Ec2Resource` component object\.

## Supported Platforms<a name="dp-resource-supported-platforms"></a>

Pipelines can launch your resources into the following platforms:

EC2\-Classic  
Your resources run in a single, flat network that you share with other customers\.

EC2\-VPC  
Your resources run in a virtual private cloud \(VPC\) that's logically isolated to your AWS account\.

Your AWS account can launch resources either into both platforms or only into EC2\-VPC, on a region by region basis\. For more information, see [Supported Platforms](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\.

If your AWS account supports only EC2\-VPC, we create a default VPC for you in each AWS Region\. By default, we launch your resources into a default subnet of your default VPC\. Alternatively, you can create a nondefault VPC and specify one of its subnets when you configure your resources, and then we launch your resources into the specified subnet of the nondefault VPC\.

When you launch an instance into a VPC, you must specify a security group created specifically for that VPC\. You can't specify a security group that you created for EC2\-Classic when you launch an instance into a VPC\. In addition, you must use the security group ID and not the security group name to identify a security group for a VPC\.

For more information about using a VPC with AWS Data Pipeline, see [Launching Resources for Your Pipeline into a VPC](dp-resources-vpc.md)\.

## Amazon EC2 Spot Instances with Amazon EMR Clusters and AWS Data Pipeline<a name="dp-emrspotinstances"></a>

Pipelines can use Amazon EC2 Spot Instances for the task nodes in their Amazon EMR cluster resources\. By default, pipelines use On\-Demand Instances\. Spot Instances let you use spare EC2 instances and run them\. The Spot Instance pricing model complements the On\-Demand and Reserved Instance pricing models, potentially providing the most cost\-effective option for obtaining compute capacity, depending on your application\. For more information, see the [Amazon EC2 Spot Instances](http://aws.amazon.com/ec2/spot-instances/) product page\.

When you use Spot Instances, AWS Data Pipeline submits your Spot Instance maximum price to Amazon EMR when your cluster is launched\. It automatically allocates the cluster's work to the number of Spot Instance task nodes that you define using the `taskInstanceCount` field\. AWS Data Pipeline limits Spot Instances for task nodes to ensure that on\-demand core nodes are available to run your pipeline\. 

You can edit a failed or completed pipeline resource instance to add Spot Instances\. When the pipeline re\-launches the cluster, it uses Spot Instances for the task nodes\.

### Spot Instances Considerations<a name="dp-emrspotinstances-considerations"></a>

When you use Spot Instances with AWS Data Pipeline, the following considerations apply:
+ Your Spot Instances can terminate when the Spot Instance price goes above your maximum price for the instance, or due to Amazon EC2 capacity reasons\. However, you do not lose your data because AWS Data Pipeline employs clusters with core nodes that are always On\-Demand Instances and not subject to termination\.
+ Spot Instances can take more time to start as they fulfill capacity asynchronously\. Therefore, a Spot Instance pipeline could run more slowly than an equivalent On\-Demand Instance pipeline\.
+ Your cluster might not run if you do not receive your Spot Instances, such as when your maximum price is too low\. 