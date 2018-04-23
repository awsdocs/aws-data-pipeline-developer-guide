# Attach EBS volumes to cluster nodes and configure a cluster in a private subnet<a name="emrcluster-example-ebs-vpc"></a>

**Example**  <a name="example8"></a>
The following example shows how to configure an Amazon EMR cluster to use Amazon EBS volumes for its nodes, and how to launch the cluster into a private subnet in a VPC\.  
These configurations are optional\. You can use them in any pipeline that uses an `EmrCluster` object\.  
+ This example of the cluster uses Amazon EBS volumes for its master, task, and core nodes\. For more information, see [Amazon EBS volumes in Amazon EMR](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-plan-storage.html) in the *Amazon EMR Management Guide*\.

  You can attach EBS volumes to any type of node in the EMR cluster within your pipeline\. To attach EBS volumes to nodes, use `coreEbsConfiguration`, `masterEbsConfiguration`, and `TaskEbsConfiguration` in your `EmrCluster` configuration\. 

  In the pipeline, click the `EmrCluster` object configuration, choose **Master EBS Configuration**, **Core EBS Configuration**, or **Task EBS Configuration**, and enter the configuration details similar to the following example\.
+ This example also includes a configuration that launches the cluster into a private subnet in a VPC\. For more information, see [Launch Amazon EMR Clusters into a VPC](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-vpc-launching-job-flows.html) in the *Amazon EMR Management Guide*\.

  To launch an Amazon EMR cluster in a private subnet, specify `SubnetId`, `emrManagedMasterSecurityGroupId`, `emrManagedSlaveSecurityGroupId`, and `ServiceAccessSecurityGroupId` in your `EmrCluster` configuration\.

```
 {
      "name": "EmrClusterForBackup",
      "coreInstanceCount": "1",
      "taskInstanceCount": "1",
      "taskInstanceType": "m4.xlarge",
      "coreInstanceType": "m4.xlarge",
      "releaseLabel": "emr-4.7.0",
      "masterInstanceType": "m4.xlarge",
      "id": "EmrClusterForBackup",
      "subnetId": "#{mySubnetId}",
      "emrManagedMasterSecurityGroupId": "#{myMasterSecurityGroup}",
      "emrManagedSlaveSecurityGroupId": "#{mySlaveSecurityGroup}",
      "serviceAccessSecurityGroupId": "#{myServiceAccessSecurityGroup}",
      "region": "#{myDDBRegion}",
      "type": "EmrCluster",
      "coreEbsConfiguration": {
        "ref": “CoreEBSConfiguration”
      },
      "masterEbsConfiguration": {
        "ref": “MasterEBSConfiguration”
      },
      "taskEbsConfiguration": {
        "ref": “TaskEBSConfiguration”
      },
      "keyPair": “sample-user-keypair"
    },
    {
       "name": “MasterEBSConfiguration”,
        "id": "MasterEBSConfiguration”,
        "ebsOptimized": "true",
        "ebsBlockDeviceConfig" : [
            { "ref": “MasterBlockDeviceConfig” }
        ],
        "type": "EbsConfiguration"
    },
    {
       "name": “TaskEBSConfiguration”,
        "id": "TaskEBSConfiguration”,
        "ebsOptimized": "true",
        "ebsBlockDeviceConfig" : [
            { "ref": “TaskBlockDeviceConfig” }
        ],
        "type": "EbsConfiguration"
    },
    {
       "name": “CoreEBSConfiguration”,
        "id": "CoreEBSConfiguration”,
        "ebsOptimized": "true",
        "ebsBlockDeviceConfig" : [
            { "ref": “CoreBlockDeviceConfig” }
        ],
        "type": "EbsConfiguration"
    },
    {
        "name": "TaskBlockDeviceConfig”,
        "id": "TaskBlockDeviceConfig”,
        "type": "EbsBlockDeviceConfig",
        "volumesPerInstance" : "2",
        "volumeSpecification" : {
            "ref": "VolumeSpecification"
        }
    },
    {
        "name": "CoreBlockDeviceConfig”,
        "id": "CoreBlockDeviceConfig”,
        "type": "EbsBlockDeviceConfig",
        "volumesPerInstance" : “4”,
        "volumeSpecification" : {
            "ref": "VolumeSpecification"
        }
    },
    {
        "name": “MasterBlockDeviceConfig”,
        "id": "MasterBlockDeviceConfig”,
        "type": "EbsBlockDeviceConfig",
        "volumesPerInstance" : “1”,
        "volumeSpecification" : {
            "ref": "VolumeSpecification"
        }
    },
    {
      "name": "VolumeSpecification",
      "id": "VolumeSpecification",
      "type": "VolumeSpecification",
      "sizeInGB": "500",
      "volumeType": "io1",
      "iops": "1000"
    }
```