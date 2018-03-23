# Interpreting Pipeline Status Codes<a name="dp-interpret-status"></a>

The status levels displayed in the AWS Data Pipeline console and CLI indicate the condition of a pipeline and its components\. The pipeline status is simply an overview of a pipeline; to see more information, view the status of individual pipeline components\.

A pipeline has a `SCHEDULED` status if it is ready \(the pipeline definition passed validation\), currently performing work, or finished performing work\. A pipeline has a `PENDING` status if it is not activated or not able to perform work \(for example, the pipeline definition failed validation\.\)

A pipeline is considered inactive if its status is `PENDING`, `INACTIVE`, or `FINISHED`\. Inactive pipelines incur a charge \(for more information, see [Pricing](https://aws.amazon.com/datapipeline/pricing)\)\.Status Codes

`ACTIVATING`  
The component or resource is being started, such as an EC2 instance\.

`CANCELED`  
The component was canceled by a user or AWS Data Pipeline before it could run\. This can happen automatically when a failure occurs in a different component or resource that this component depends on\.

`CASCADE_FAILED`  
The component or resource was canceled as a result of a cascade failure from one of its dependencies, but the component was probably not the original source of the failure\.

`DEACTIVATING`  
The pipeline is being deactivated\.

`FAILED`  
The component or resource encountered an error and stopped working\. When a component or resource fails, it can cause cancelations and failures to cascade to other components that depend on it\.

`FINISHED`  
The component completed its assigned work\.

`INACTIVE`  
The pipeline was deactivated\.

`PAUSED`  
The component was paused and is not currently performing its work\.

`PENDING`  
The pipeline is ready to be activated for the first time\.

`RUNNING`  
The resource is running and ready to receive work\.

`SCHEDULED`  
The resource is scheduled to run\.

`SHUTTING_DOWN`  
The resource is shutting down after successfully completing its work\.

`SKIPPED`  
The component skipped intervals of execution after the pipeline was activated using a time stamp that is later than the current schedule\.

`TIMEDOUT`  
The resource exceeded the `terminateAfter` threshold and was stopped by AWS Data Pipeline\. After the resource reaches this status, AWS Data Pipeline ignores the `actionOnResourceFailure`, `retryDelay`, and `retryTimeout` values for that resource\. This status applies only to resources\.

`VALIDATING`  
The pipeline definition is being validated by AWS Data Pipeline\.

`WAITING_FOR_RUNNER`  
The component is waiting for its worker client to retrieve a work item\. The component and worker client relationship is controlled by the `runsOn` or `workerGroup` fields defined by that component\.

`WAITING_ON_DEPENDENCIES`  
The component is verifying that its default and user\-configured preconditions are met before performing its work\.