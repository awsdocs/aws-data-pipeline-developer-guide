# Schedule<a name="dp-object-schedule"></a>

Defines the timing of a scheduled event, such as when an activity runs\.

**Note**  
When a schedule's start time is in the past, AWS Data Pipeline backfills your pipeline and begins scheduling runs immediately beginning at the specified start time\. For testing/development, use a relatively short interval\. Otherwise, AWS Data Pipeline attempts to queue and schedule all runs of your pipeline for that interval\. AWS Data Pipeline attempts to prevent accidental backfills if the pipeline component `scheduledStartTime` is earlier than 1 day ago by blocking pipeline activation\.

## Examples<a name="schedule-example"></a>

The following is an example of this object type\. It defines a schedule of every hour starting at 00:00:00 hours on 2012\-09\-01 and ending at 00:00:00 hours on 2012\-10\-01\. The first period ends at 01:00:00 on 2012\-09\-01\.

```
{
  "id" : "Hourly",
  "type" : "Schedule",
  "period" : "1 hours",
  "startDateTime" : "2012-09-01T00:00:00",
  "endDateTime" : "2012-10-01T00:00:00"
}
```

The following pipeline will start at the `FIRST_ACTIVATION_DATE_TIME` and run every hour until 22:00:00 hours on 2014\-04\-25\.

```
{
     "id": "SchedulePeriod",
     "name": "SchedulePeriod",
     "startAt": "FIRST_ACTIVATION_DATE_TIME",
     "period": "1 hours",
     "type": "Schedule",
     "endDateTime": "2014-04-25T22:00:00"
   }
```

The following pipeline will start at the `FIRST_ACTIVATION_DATE_TIME` and run every hour and complete after three occurrences\.

```
{
     "id": "SchedulePeriod",
     "name": "SchedulePeriod",
     "startAt": "FIRST_ACTIVATION_DATE_TIME",
     "period": "1 hours",
     "type": "Schedule",
     "occurrences": "3"
   }
```

The following pipeline will start at 22:00:00 on 2014\-04\-25, run hourly, and end after three occurrences\.

```
{
     "id": "SchedulePeriod",
     "name": "SchedulePeriod",
     "startDateTime": "2014-04-25T22:00:00",
     "period": "1 hours",
     "type": "Schedule",
     "occurrences": "3"
   }
```

On\-demand using the Default object

```
{
  "name": "Default",
  "resourceRole": "DataPipelineDefaultResourceRole",
  "role": "DataPipelineDefaultRole",
  "scheduleType": "ondemand"
}
```

On\-demand with explicit Schedule object

```
{
  "name": "Default",
  "resourceRole": "DataPipelineDefaultResourceRole",
  "role": "DataPipelineDefaultRole",
  "scheduleType": "ondemand"
},
{
  "name": "DefaultSchedule",
  "type": "Schedule",
  "id": "DefaultSchedule",
  "period": "ONDEMAND_PERIOD",
  "startAt": "ONDEMAND_ACTIVATION_TIME"
},
```

The following examples demonstrate how a Schedule can be inherited from the default object, be explicitly set for that object, or be given by a parent reference:

Schedule inherited from Default object

```
{
  "objects": [
  {       
      "id": "Default",
      "failureAndRerunMode":"cascade",
      "resourceRole": "DataPipelineDefaultResourceRole",
      "role": "DataPipelineDefaultRole",
      "pipelineLogUri": "s3://myLogsbucket",
      "scheduleType": "cron",
      "schedule": {
        "ref": "DefaultSchedule"
      }
   },
   {
      "type": "Schedule",
      "id": "DefaultSchedule",
      "occurrences": "1",
      "period": "1 Day",
      "startAt": "FIRST_ACTIVATION_DATE_TIME"
    },
    { 
      "id": "A_Fresh_NewEC2Instance",
      "type": "Ec2Resource",
      "terminateAfter": "1 Hour"
    },
    {
      "id": "ShellCommandActivity_HelloWorld",
      "runsOn": {
        "ref": "A_Fresh_NewEC2Instance"
      },
      "type": "ShellCommandActivity",
      "command": "echo 'Hello World!'"
    }
  ]
}
```

Explicit schedule on the object

```
{
  "objects": [
  {       
      "id": "Default",
      "failureAndRerunMode":"cascade",
      "resourceRole": "DataPipelineDefaultResourceRole",
      "role": "DataPipelineDefaultRole",
      "pipelineLogUri": "s3://myLogsbucket",
      "scheduleType": "cron"
      
   },
   {
      "type": "Schedule",
      "id": "DefaultSchedule",
      "occurrences": "1",
      "period": "1 Day",
      "startAt": "FIRST_ACTIVATION_DATE_TIME"
    },
    { 
      "id": "A_Fresh_NewEC2Instance",
      "type": "Ec2Resource",
      "terminateAfter": "1 Hour"
    },
    {
      "id": "ShellCommandActivity_HelloWorld",
      "runsOn": {
        "ref": "A_Fresh_NewEC2Instance"
      },
      "schedule": {
        "ref": "DefaultSchedule"
      },
      "type": "ShellCommandActivity",
      "command": "echo 'Hello World!'"
    }
  ]
}
```

Schedule from Parent reference

```
{
  "objects": [
  {       
      "id": "Default",
      "failureAndRerunMode":"cascade",
      "resourceRole": "DataPipelineDefaultResourceRole",
      "role": "DataPipelineDefaultRole",
      "pipelineLogUri": "s3://myLogsbucket",
      "scheduleType": "cron"
      
   },
   {       
      "id": "parent1",
      "schedule": {
        "ref": "DefaultSchedule"
      }
      
   },
   {
      "type": "Schedule",
      "id": "DefaultSchedule",
      "occurrences": "1",
      "period": "1 Day",
      "startAt": "FIRST_ACTIVATION_DATE_TIME"
    },
    { 
      "id": "A_Fresh_NewEC2Instance",
      "type": "Ec2Resource",
      "terminateAfter": "1 Hour"
    },
    {
      "id": "ShellCommandActivity_HelloWorld",
      "runsOn": {
        "ref": "A_Fresh_NewEC2Instance"
      },
      "parent": {
        "ref": "parent1"
      },
      "type": "ShellCommandActivity",
      "command": "echo 'Hello World!'"
    }
  ]
}
```

## Syntax<a name="schedule-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| period | How often the pipeline should run\. The format is "N \[minutes\|hours\|days\|weeks\|months\]", where N is a number followed by one of the time specifiers\. For example, "15 minutes", runs the pipeline every 15 minutes\. The minimum period is 15 minutes and the maximum period is 3 years\. | Period | 


****  

| Required Group \(One of the following is required\) | Description | Slot Type | 
| --- | --- | --- | 
| startAt | The date and time at which to start the scheduled pipeline runs\. Valid value is FIRST\_ACTIVATION\_DATE\_TIME, which is deprecated in favor of creating an on\-demand pipeline\. | Enumeration | 
| startDateTime | The date and time to start the scheduled runs\. You must use either startDateTime or startAt but not both\. | DateTime | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| endDateTime | The date and time to end the scheduled runs\. Must be a date and time later than the value of startDateTime or startAt\. The default behavior is to schedule runs until the pipeline is shut down\.  | DateTime | 
| occurrences | The number of times to execute the pipeline after it's activated\. You can't use occurrences with endDateTime\. | Integer | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @version | Pipeline version the object was created with\. | String | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object | String | 
| @firstActivationTime | The time of object creation\. | DateTime | 
| @pipelineId | Id of the pipeline to which this object belongs to | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects | String | 