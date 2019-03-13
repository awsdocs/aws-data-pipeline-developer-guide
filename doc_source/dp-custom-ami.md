# Task Runner and Custom AMIs<a name="dp-custom-ami"></a>

When you specify an `Ec2Resource` object for your pipeline, AWS Data Pipeline creates an EC2 instance for you, using an AMI that installs and configures Task Runner for you\. A PV\-compatible instance type is required in this case\. Alternatively, you can create a custom AMI with Task Runner, and then specify the ID of this AMI using the `imageId` field of the `Ec2Resource` object\. For more information, see [Ec2Resource](dp-object-ec2resource.md)\.

A custom AMI must meet the following requirements for AWS Data Pipeline to use it successfully for Task Runner:
+ Create the AMI in the same region in which the instances will run\. For more information, see [Creating Your Own AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ Ensure that the virtualization type of the AMI is supported by the instance type you plan to use\. For example, the I2 and G2 instance types require an HVM AMI and the T1, C1, M1, and M2 instance types require a PV AMI\. For more information, see [Linux AMI Virtualization Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ Install the following software:
  + Linux
  + Bash
  + wget
  + unzip
  + Java 1\.6 or 1\.8
  + cloud\-init
+ Create and configure a user account named `ec2-user`\.