# Resource<a name="dp-copydata-redshift-resource-cli"></a>

This is a definition of the computational resource that performs the copy operation\. In this example, AWS Data Pipeline should automatically create an EC2 instance to perform the copy task and terminate the instance after the task completes\. The fields defined here control the creation and function of the instance that does the work\. For more information, see [Ec2Resource](dp-object-ec2resource.md)\.

The `Ec2Resource` is defined by the following fields:

```
{
  "id": "Ec2ResourceId1",
  "schedule": {
    "ref": "ScheduleId1"
  },
  "securityGroups": "MySecurityGroup",
  "name": "DefaultEc2Resource1",
  "role": "DataPipelineDefaultRole",
  "logUri": "s3://myLogs",
  "resourceRole": "DataPipelineDefaultResourceRole",
  "type": "Ec2Resource"
},
```

`id`  
The user\-defined ID, which is a label for your reference only\.

`schedule`  
The schedule on which to create this computational resource\.

`securityGroups`  
The security group to use for the instances in the resource pool\.

`name`  
The user\-defined name, which is a label for your reference only\.

`role`  
The IAM role of the account that accesses resources, such as accessing an Amazon S3 bucket to retrieve data\.

`logUri`  
The Amazon S3 destination path to back up Task Runner logs from the `Ec2Resource`\.

`resourceRole`  
The IAM role of the account that creates resources, such as creating and configuring an EC2 instance on your behalf\. Role and ResourceRole can be the same role, but separately provide greater granularity in your security configuration\.