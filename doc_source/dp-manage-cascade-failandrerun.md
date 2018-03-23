# Cascading Failures and Reruns<a name="dp-manage-cascade-failandrerun"></a>

AWS Data Pipeline allows you to configure the way pipeline objects behave when a dependency fails or is canceled by a user\. You can ensure that failures cascade to other pipeline objects \(consumers\), to prevent indefinite waiting\. All activities, data nodes, and preconditions have a field named `failureAndRerunMode` with a default value of `none`\. To enable cascading failures, set the `failureAndRerunMode` field to `cascade`\. 

When this field is enabled, cascade failures occur if a pipeline object is blocked in the `WAITING_ON_DEPENDENCIES` state and any dependencies have failed with no pending command\. During a cascade failure, the following events occur:
+ When an object fails, its consumers are set to `CASCADE_FAILED` and both the original object and its consumers' preconditions are set to `CANCELED`\.
+ Any objects that are already `FINISHED`, `FAILED`, or `CANCELED` are ignored\.

Cascade failure does not operate on a failed object's dependencies \(upstream\), except for preconditions associated with the original failed object\. Pipeline objects affected by a cascade failure may trigger any retries or post\-actions, such as `onFail`\.

The detailed effects of a cascading failure depend on the object type\.

## Activities<a name="dp-manage-cascade-activity"></a>

An activity changes to `CASCADE_FAILED` if any of its dependencies fail, and it subsequently triggers a cascade failure in the activity's consumers\. If a resource fails that the activity depends on, the activity is `CANCELED` and all its consumers change to `CASCADE_FAILED`\.

## Data Nodes and Preconditions<a name="dp-manage-cascade-datanode"></a>

If a data node is configured as the output of an activity that fails, the data node changes to the `CASCADE_FAILED` state\. The failure of a data node propagates to any associated preconditions, which change to the `CANCELED` state\.

## Resources<a name="dp-manage-cascade-resources"></a>

If the objects that depend on a resource are in the `FAILED` state and the resource itself is in the `WAITING_ON_DEPENDENCIES` state, then the resource changes to the `FINISHED` state\.

## Rerunning Cascade\-Failed Objects<a name="dp-manage-cascade-rerun"></a>

By default, rerunning any activity or data node only reruns the associated resource\. However, setting the `failureAndRerunMode` field to `cascade` on a pipeline object allows a rerun command on a target object to propagate to all consumers, under the following conditions: 
+ The target object's consumers are in the `CASCADE_FAILED` state\.
+ The target object's dependencies have no rerun commands pending\.
+ The target object's dependencies are not in the `FAILED`, `CASCADE_FAILED`, or `CANCELED` state\.

If you attempt to rerun a `CASCADE_FAILED` object and any of its dependencies are `FAILED`, `CASCADE_FAILED`, or `CANCELED`, the rerun will fail and return the object to the `CASCADE_FAILED` state\. To successfully rerun the failed object, you must trace the failure up the dependency chain to locate the original source of failure and rerun that object instead\. When you issue a rerun command on a resource, you also attempt to rerun any objects that depend on it\. 

## Cascade\-Failure and Backfills<a name="dp-manage-cascade-backfills"></a>

If you enable cascade failure and have a pipeline that creates many backfills, pipeline runtime errors can cause resources to be created and deleted in rapid succession without performing useful work\. AWS Data Pipeline attempts to alert you about this situation with the following warning message when you save a pipeline: ` Pipeline_object_name has 'failureAndRerunMode' field set to 'cascade' and you are about to create a backfill with scheduleStartTime start_time. This can result in rapid creation of pipeline objects in case of failures. ` This happens because cascade failure can quickly set downstream activities as `CASCADE_FAILED` and shut down EMR clusters and EC2 resources that are no longer needed\. We recommended that you test pipelines with short time ranges to limit the effects of this situation\. 