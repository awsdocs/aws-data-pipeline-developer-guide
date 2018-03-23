# Getting Started Using ShellCommandActivity<a name="dp-template-gettingstartedshell"></a>

The **Getting Started using ShellCommandActivity** template runs a shell command script to count the number of GET requests in a log file\. The output is written in a time\-stamped Amazon S3 location on every scheduled run of the pipeline\.

The template uses the following pipeline objects:
+ ShellCommandActivity
+ S3InputNode
+ S3OutputNode
+ Ec2Resource