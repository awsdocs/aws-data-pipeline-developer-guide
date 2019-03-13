# Launching Resources for Your Pipeline into a VPC<a name="dp-resources-vpc"></a>

Pipelines can launch Amazon EC2 instances and Amazon EMR clusters into a virtual private cloud \(VPC\)\. 
+ First, create a VPC and subnets using Amazon VPC\. Configure the VPC so that instances in the VPC can access AWS Data Pipeline endpoint and Amazon S3\.
+ Next, set up a security group that grants Task Runner access to your data sources\.
+ Finally, specify a subnet from the VPC when you configure your instances and clusters and when you create your data sources\.

Note that if you have a default VPC in a region, it's already configured to access other AWS services\. When you launch a resource, we'll automatically launch it into your default VPC\.

For more information about VPCs, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

**Topics**
+ [Create and Configure a VPC](#dp-create-vpc)
+ [Set up Connectivity Between Resources](#dp-vpc-security-groups)
+ [Configure the Resource](#dp-configure-resource)

## Create and Configure a VPC<a name="dp-create-vpc"></a>

A VPC that you create must have a subnet, an internet gateway, and a route table for the subnet with a route to the internet gateway so that instances in the VPC can access Amazon S3\. \(If you have a default VPC, it is already configured this way\.\) The easiest way to create and configure your VPC is to use the VPC wizard, as shown in the following procedure\.

**To create and configure your VPC using the VPC wizard**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. From the navigation bar, use the region selector to select the region for your VPC\. You launch all instances and clusters into this VPC, so select the region that makes sense for your pipeline\.

1. Click **VPC Dashboard** in the navigation pane\.

1. Locate the **Your Virtual Private Cloud** area of the dashboard and click **Get started creating a VPC**, if you have no VPC resources, or click **Start VPC Wizard**\.

1. Select the first option, **VPC with a Single Public Subnet Only**, and then click **Continue**\.

1. The confirmation page shows the CIDR ranges and settings that you've chosen\. Verify that **Enable DNS hostnames** is **Yes**\. Make any other changes that you need, and then click **Create VPC** to create your VPC, subnet, internet gateway, and route table\.

1. After the VPC is created, click **Your VPCs** in the navigation pane and select your VPC from the list\.
   + On the **Summary** tab, make sure that both **DNS resolution** and **DNS hostnames** are **yes**\.
   + Click the identifier for the DHCP options set\. Make sure that **domain\-name\-servers** is `AmazonProvidedDNS` and **domain\-name** is `ec2.internal` for the US East \(N\. Virginia\) region and *region\-name*\.`compute.internal` for all other regions\. Otherwise, create a new options set with these settings and associate it with the VPC\. For more information, see [Working with DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html#DHCPOptionSet) in the *Amazon VPC User Guide*\.

If you prefer to create the VPC, subnet, internet gateway, and route table manually, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#Create-VPC) and [Adding an Internet Gateway to Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#Create-VPC) in the *Amazon VPC User Guide*\.

## Set up Connectivity Between Resources<a name="dp-vpc-security-groups"></a>

Security groups act as a virtual firewall for your instances to control inbound and outbound traffic\. You must grant Task Runner access to your data sources\.

For more information about security groups, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

First, identify the security group or IP address used by the resource running Task Runner\.
+ If your resource is of type [EmrCluster](dp-object-emrcluster.md), Task Runner runs on the cluster by default\. We create security groups named `ElasticMapReduce-master` and `ElasticMapReduce-slave` when you launch the cluster\. You need the IDs of these security groups later on\.

**To get the IDs of the security groups for a cluster in a VPC**

  1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

  1. In the navigation pane, click **Security Groups**\.

  1. If you have a lengthy list of security groups, you can click the **Name** column to sort your security groups by name\. If you don't see a **Name** column, click the **Show/Hide Columns** icon, and then click **Name**\.

  1. Note the IDs of the `ElasticMapReduce-master` and `ElasticMapReduce-slave` security groups\.
+ If your resource is of type [Ec2Resource](dp-object-ec2resource.md), Task Runner runs on the EC2 instance by default\. Create a security group for the VPC and specify it when you launch the EC2 instance\. You need the ID of this security group later on\.

**To create a security group for an EC2 instance in a VPC**

  1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

  1. In the navigation pane, click **Security Groups**\.

  1. Click **Create Security Group**\.

  1. Specify a name and description for the security group\.

  1. Select your VPC from the list, and then click **Create**\.

  1. Note the ID of the new security group\.
+ If you are running Task Runner on your own computer, note its public IP address, in CIDR notation\. If the computer is behind a firewall, note the entire address range of its network\. You need this address later on\.

Next, create rules in the resource security groups that allow inbound traffic for the data sources Task Runner must access\. For example, if Task Runner must access an Amazon Redshift cluster, the security group for the Amazon Redshift cluster must allow inbound traffic from the resource\.

**To add a rule to the security group for an Amazon RDS database**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, click **Instances**\.

1. Click the details icon for the DB instance\. Under **Security and Network**, click the link to the security group, which takes you to the Amazon EC2 console\. If you're using the old console design for security groups, switch to the new console design by clicking the icon that's displayed at the top of the console page\.

1. From the **Inbound** tab, click **Edit** and then click **Add Rule**\. Specify the database port that you used when you launched the DB instance\. Start typing the ID of the security group or IP address used by the resource running Task Runner in **Source**\.

1. Click **Save**\.

**To add a rule to the security group for an Amazon Redshift cluster**

1. Open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, click **Clusters**\.

1. Click the details icon for the cluster\. Under **Cluster Properties**, note the name or ID of the security group, and then click **View VPC Security Groups**, which takes you to the Amazon EC2 console\. If you're using the old console design for security groups, switch to the new console design by clicking the icon that's displayed at the top of the console page\.

1. Select the security group for the cluster\.

1. From the **Inbound** tab, click **Edit** and then click **Add Rule**\. Specify the type, protocol, and port range\. Start typing the ID of the security group or IP address used by the resource running Task Runner in **Source**\.

1. Click **Save**\.

## Configure the Resource<a name="dp-configure-resource"></a>

To launch a resource into a subnet of a nondefault VPC or a nondefault subnet of a default VPC, you must specify the subnet using the `subnetId` field when you configure the resource\. If you have a default VPC and you don't specify `subnetId`, we launch the resource into the default subnet of the default VPC\.

### Example EmrCluster<a name="dp-emrcluster"></a>

The following example object launches an Amazon EMR cluster into a nondefault VPC\.

```
{
  "id" : "MyEmrCluster",
  "type" : "EmrCluster",
  "keyPair" : "my-key-pair",
  "masterInstanceType" : "m1.xlarge",
  "coreInstanceType" : "m1.small",
  "coreInstanceCount" : "10",
  "taskInstanceType" : "m1.small",
  "taskInstanceCount": "10",
  "subnetId": "subnet-12345678"
}
```

For more information, see [EmrCluster](dp-object-emrcluster.md)\.

### Example Ec2Resource<a name="dp-ec2resource"></a>

The following example object launches an EC2 instance into a nondefault VPC\. Notice that you must specify security groups for an instance in a nondefault VPC using their IDs, not their names\.

```
{
  "id" : "MyEC2Resource",
  "type" : "Ec2Resource",
  "actionOnTaskFailure" : "terminate",
  "actionOnResourceFailure" : "retryAll",
  "maximumRetries" : "1",
  "role" : "test-role",
  "resourceRole" : "test-role",
  "instanceType" : "m1.medium",
  "securityGroupIds" : "sg-12345678",
  "subnetId": "subnet-1a2b3c4d",
  "associatePublicIpAddress": "true",
  "keyPair" : "my-key-pair"
}
```

For more information, see [Ec2Resource](dp-object-ec2resource.md)\.