# \(Optional\) Granting Task Runner Access to Amazon RDS<a name="dp-taskrunner-rdssecurity"></a>

Amazon RDS allows you to control access to your DB instances using database security groups \(DB security groups\)\. A DB security group acts like a firewall controlling network access to your DB instance\. By default, network access is turned off for your DB instances\. You must modify your DB security groups to let Task Runner access your Amazon RDS instances\. Task Runner gains Amazon RDS access from the instance on which it runs, so the accounts and security groups that you add to your Amazon RDS instance depend on where you install Task Runner\. 

**To grant access to Task Runner in EC2\-Classic**

1. Open the Amazon RDS console\.

1. In the navigation pane, choose **Instances**, and then select your DB instance\.

1. Under **Security and Network**, select the security group, which opens the **Security Groups** page with this DB security group selected\. Select the details icon for the DB security group\.

1. Under **Security Group Details**, create a rule with the appropriate **Connection Type** and **Details**\. These fields depend on where Task Runner is running, as described here:
   + `Ec2Resource`
     + **Connection Type**: `EC2 Security Group`

       **Details**: *my\-security\-group\-name* \(the name of the security group you created for the EC2 instance\)
   + `EmrResource`
     + **Connection Type**: `EC2 Security Group`

       **Details**: `ElasticMapReduce-master`
     + **Connection Type**: `EC2 Security Group`

       **Details**: `ElasticMapReduce-slave`
   + Your local environment \(on\-premises\)
     + **Connection Type**: `CIDR/IP`:

       **Details**: *my\-ip\-address* \(the IP address of your computer or the IP address range of your network, if your computer is behind a firewall\)

1. Click Add\.

**To grant access to Task Runner in EC2\-VPC**

1. Open the Amazon RDS console\.

1. In the navigation pane, choose **Instances**\.

1. Select the details icon for the DB instance\. Under **Security and Network**, open the link to the security group, which takes you to the Amazon EC2 console\. If you're using the old console design for security groups, switch to the new console design by selecting the icon that's displayed at the top of the console page\.

1. On the **Inbound** tab, choose **Edit**, **Add Rule**\. Specify the database port that you used when you launched the DB instance\. The source depends on where Task Runner is running, as described here:
   + `Ec2Resource`
     + *my\-security\-group\-id* \(the ID of the security group you created for the EC2 instance\)
   + `EmrResource`
     + *master\-security\-group\-id* \(the ID of the `ElasticMapReduce-master` security group\)
     + *slave\-security\-group\-id* \(the ID of the `ElasticMapReduce-slave` security group\)
   + Your local environment \(on\-premises\)
     + *ip\-address* \(the IP address of your computer or the IP address range of your network, if your computer is behind a firewall\)

1. Click **Save**\.