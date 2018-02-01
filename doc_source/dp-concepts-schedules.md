# Scheduling Pipelines<a name="dp-concepts-schedules"></a>

In AWS Data Pipeline, a schedule defines the timing of a scheduled event, such as when an activity runs\. AWS Data Pipeline exposes this functionality through the [Schedule](dp-object-schedule.md) pipeline component\. 

## Creating a Schedule Using the Console<a name="dp-concepts-creating-schedule"></a>

The AWS Data Pipeline console allows you to schedule and create pipelines\. This is useful for testing and prototyping pipelines before establishing them for production workloads\.

The **Create Pipeline** section has the following fields:


| Field | Action | 
| --- | --- | 
| Name | Enter a name for the pipeline\. | 
| Description |  \(Optional\) Enter a description for the pipeline\.  | 

The **Schedule** section has the following fields:


| Field | Action | 
| --- | --- | 
| Run |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-concepts-schedules.html)  | 
| Run every  | Enter a period for every pipeline run\. | 
| Starting | Enter a time and date for the pipeline to start\. Alternatively, your start date and time are automatically selected at pipeline activation\. | 
| Ending | Enter a time and date for the pipeline to end\. If you select never, your pipeline continues to execute indefinitely\. | 

The **IAM Roles & Permissions** section has the following options:


| Field | Action | 
| --- | --- | 
| Default | Choose this to have AWS Data Pipeline determine the roles for you\. | 
| Custom | Choose this to designate your own IAM roles\. If you select this option, you can choose the following roles: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-concepts-schedules.html) | 

## On\-Demand<a name="dp-concepts-ondemand"></a>

**Note**  
You can find the Default object on the Architect page in the **Other** section\.

AWS Data Pipeline offers an on\-demand schedule type, which gives the option for a pipeline to be run on pipeline activation\. The pipeline is run one time in response to an activation request\.

On\-demand pipelines only require the schedule type to be set to `ondemand` on the default object\. On\-demand pipelines require that you *not* use a schedule object and they do not allow for multiple schedules\. The maximum number of concurrent executions of an on\-demand pipeline can be configured using the slot maxActiveInstances in the Default object\. The default value for this slot is 1 for on\-demand pipelines and can have a maximum value of 5\.

The following Default object uses on\-demand scheduling:

```
{
  "name": "Default",
  "resourceRole": "DataPipelineDefaultResourceRole",
  "role": "DataPipelineDefaultRole",
  "scheduleType": "ondemand"
}
```

## Time Series Style vs\. Cron Style<a name="dp-concepts-timeseries-cron"></a>

AWS Data Pipeline offers two types of periodic pipeline component scheduling: time series style scheduling and cron style scheduling\. 

The schedule type allows you to specify whether the pipeline component instances should start at the beginning of the interval \(also known as the period\) or at the end of the interval\. 

Time series style scheduling means that instances are scheduled at the end of each interval and cron style scheduling means that instances are scheduled at the beginning of each interval\. For example, using time series style scheduling, if the start time is 22:00 UTC and the interval/period is set to 30 minutes, then the pipeline component instance's first run starts at 22:30 UTC, not 22:00 UTC\. If you want the instance to run at the beginning of the period/interval, such as 22:00 UTC, use cron style scheduling instead\.

**Note**  
The minimum scheduling interval is 15 minutes\.

**Note**  
You can find the Default object on the Architect page in the **Other** section\.

The following is a Default object for cron style pipelines:

```
{
  "name": "Default",
  "resourceRole": "DataPipelineDefaultResourceRole",
 "role": "DataPipelineDefaultRole",
  "scheduleType": "cron"
}
```

The following is a Default object for time series style pipelines:

```
{
  "name": "Default",
  "resourceRole": "DataPipelineDefaultResourceRole",
  "role": "DataPipelineDefaultRole",
  "scheduleType": "timeseries"
}
```

### Resources Ignore Schedule Type<a name="dp-concepts-resources-special-schedule"></a>

 AWS Data Pipeline creates activity and data node instances at the beginning or end of the schedule interval depending on the pipeline's schedule type setting \(time series style scheduling or cron style scheduling\)\. However, AWS Data Pipeline creates `Resource` instances, such as `EC2Resource` and `EmrCluster`, at the beginning of the interval regardless of the pipeline schedule type and sets them to the WAITING\_ON\_DEPENDENCIES status\. The actual underlying resources are not instantiated until an associated activity is scheduled\. 

## Backfill Tasks<a name="dp-concepts-backfills"></a>

When you define a pipeline with a scheduled start time for the past, AWS Data Pipeline backfills the tasks in the pipeline\. In that situation, AWS Data Pipeline immediately runs many instances of the tasks in the pipeline to catch up to the number of times those tasks would have run between the scheduled start time and the current time\. When this happens, you see pipeline component instances running back\-to\-back at a greater frequency than the period value that you specified when you created the pipeline\. AWS Data Pipeline returns your pipeline to the defined period only when it catches up to the number of past runs\. 

To minimize backfills in your development and testing phases, use a relatively short interval for `startDateTime`\.\.`endDateTime`\.

AWS Data Pipeline attempts to prevent accidental backfills by blocking pipeline activation if the pipeline component scheduledStartTime is earlier than 1 day ago\.

To get your pipeline to launch immediately, set **Start Date Time** to a date one day in the past\. AWS Data Pipeline starts launching the "past due" runs immediately in an attempt to address what it perceives as a backlog of work\. This backfilling means that you don't have to wait an hour to see AWS Data Pipeline launch its first cluster\. 

## Maximum Resource Efficiency Using Schedules<a name="dp-concepts-schedule-resources"></a>

AWS Data Pipeline allows you to maximize the efficiency of resources by supporting different schedule periods for a resource and an associated activity\. 

For example, consider an activity with a 20\-minute schedule period\. If the activity's resource were also configured for a 20\-minute schedule period, AWS Data Pipeline would create three instances of the resource in an hour and consume triple the resources necessary for the task\. 

Instead, AWS Data Pipeline lets you configure the resource with a different schedule; for example, a one\-hour schedule\. When paired with an activity on a 20\-minute schedule, AWS Data Pipeline creates only one resource to service all three instances of the activity in an hour, thus maximizing usage of the resource\.

## Protecting Against Overwriting Data<a name="dp-overwrite-data"></a>

Consider a recurring import job using AWS Data Pipeline that runs multiple times per day and routes the output to the same Amazon S3 location for each run\. 

You could accidentally overwrite your output data, unless you use a date\-based expression\. A date\-based expression such as `s3://myBucket/#{@scheduledStartTime}` for your `S3Output.DirectoryPath` can specify a separate directory path for each period\. For more information, see [Schedule](dp-object-schedule.md)\.