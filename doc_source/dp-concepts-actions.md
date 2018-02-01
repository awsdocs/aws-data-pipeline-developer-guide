# Actions<a name="dp-concepts-actions"></a>

AWS Data Pipeline actions are steps that a pipeline component takes when certain events occur, such as success, failure, or late activities\. The event field of an activity refers to an action, such as a reference to `snsalarm` in the `onLateAction` field of `EmrActivity`\. 

 AWS Data Pipeline relies on Amazon SNS notifications as the primary way to indicate the status of pipelines and their components in an unattended manner\. For more information, see [Amazon SNS](https://aws.amazon.com/sns/)\. In addition to SNS notifications, you can use the AWS Data Pipeline console and CLI to obtain pipeline status information\.

AWS Data Pipeline supports the following actions: 

[SnsAlarm](dp-object-snsalarm.md)  
An action that sends an SNS notification to a topic based on `onSuccess`, `OnFail`, and `onLateAction` events\.

[Terminate](dp-object-terminate.md)  
An action that triggers the cancellation of a pending or unfinished activity, resource, or data node\. You cannot terminate actions that include `onSuccess`, `OnFail`, or `onLateAction`\.

## Proactively Monitoring Pipelines<a name="dp-monitor-pipelines"></a>

The best way to detect problems is to monitor your pipelines proactively from the start\. You can configure pipeline components to inform you of certain situations or events, such as when a pipeline component fails or doesn't begin by its scheduled start time\. AWS Data Pipeline makes it easy to configure notifications by providing event fields on pipeline components that you can associate with Amazon SNS notifications, such as `onSuccess`, `OnFail`, and `onLateAction`\.