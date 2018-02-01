# Using Amazon EC2 Spot Instances in a Pipeline<a name="dp-spot-instances"></a>

Pipelines can use Amazon EC2 Spot Instances for the task nodes in their Amazon EMR cluster resources\. By default, pipelines use on\-demand Amazon EC2 instances\. In addition, you can use Spot Instances\. Spot Instances let you use spare Amazon EC2 instances and run them\. The Spot Instance pricing model complements the on\-demand and Reserved Instance pricing models, potentially providing the most cost\-effective option for obtaining compute capacity, depending on your application\. For more information, see [Amazon EC2 Spot Instances](http://aws.amazon.com/ec2/spot-instances/) on the *Amazon EC2 Product Page*\. 

**To use Spot Instances in your pipeline**

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. Open your pipeline in Architect\.

1. In the **Resources** pane, go to the EMR cluster\. In **Add an optional field**, select **Task Instance Bid Price**\. Set **Task Instance Bid Price** to your maximum price for a Spot Instance per hour\. This is the maximum dollar amount you pay, and is a decimal value between 0 and 20\.00 exclusive\.

For more information, see [EmrCluster](dp-object-emrcluster.md)\.