# SnsAlarm<a name="dp-object-snsalarm"></a>

 Sends an Amazon SNS notification message when an activity fails or finishes successfully\. 

## Example<a name="snsalarm-example"></a>

The following is an example of this object type\. The values for `node.input` and `node.output` come from the data node or activity that references this object in its `onSuccess` field\. 

```
{
  "id" : "SuccessNotify",
  "name" : "SuccessNotify",
  "type" : "SnsAlarm",
  "topicArn" : "arn:aws:sns:us-east-1:28619EXAMPLE:ExampleTopic",
  "subject" : "COPY SUCCESS: #{node.@scheduledStartTime}",
  "message" : "Files were copied from #{node.input} to #{node.output}."
}
```

## Syntax<a name="snsalarm-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| message | The body text of the Amazon SNS notification\. | String | 
| role | The IAM role to use to create the Amazon SNS alarm\. | String | 
| subject | The subject line of the Amazon SNS notification message\. | String | 
| topicArn | The destination Amazon SNS topic ARN for the message\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| node | The node for which this action is being performed\. | Reference Object, e\.g\. "node":\{"ref":"myRunnableObjectId"\} | 
| @version | Pipeline version the object was created with\. | String | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | Id of the pipeline to which this object belongs to\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects\. | String | 