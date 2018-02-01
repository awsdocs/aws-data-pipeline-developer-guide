# Activity<a name="dp-copydata-redshift-activity-cli"></a>

The last section in the JSON file is the definition of the activity that represents the work to perform\. In this case, we use a `RedshiftCopyActivity` component to copy data from Amazon S3 to Amazon Redshift\. For more information, see [RedshiftCopyActivity](dp-object-redshiftcopyactivity.md)\.

The `RedshiftCopyActivity` component is defined by the following fields:

```
{
  "id": "RedshiftCopyActivityId1",
  "input": {
    "ref": "S3DataNodeId1"
  },
  "schedule": {
    "ref": "ScheduleId1"
  },
  "insertMode": "KEEP_EXISTING",
  "name": "DefaultRedshiftCopyActivity1",
  "runsOn": {
    "ref": "Ec2ResourceId1"
  },
  "type": "RedshiftCopyActivity",
  "output": {
    "ref": "RedshiftDataNodeId1"
  }
},
```

`id`  
The user\-defined ID, which is a label for your reference only\.

`input`  
A reference to the Amazon S3 source file\.

`schedule`  
The schedule on which to run this activity\.

`insertMode`  
The insert type \(`KEEP_EXISTING`, `OVERWRITE_EXISTING`, or `TRUNCATE`\)\.

`name`  
The user\-defined name, which is a label for your reference only\.

`runsOn`  
The computational resource that performs the work that this activity defines\.

`output`  
A reference to the Amazon Redshift destination table\.