# Task Runners<a name="dp-how-remote-taskrunner-client"></a>

 A task runner is an application that polls AWS Data Pipeline for tasks and then performs those tasks\. 

 Task Runner is a default implementation of a task runner that is provided by AWS Data Pipeline\. When Task Runner is installed and configured, it polls AWS Data Pipeline for tasks associated with pipelines that you have activated\. When a task is assigned to Task Runner, it performs that task and reports its status back to AWS Data Pipeline\. 

The following diagram illustrates how AWS Data Pipeline and a task runner interact to process a scheduled task\. A task is a discrete unit of work that the AWS Data Pipeline service shares with a task runner\. It differs from a pipeline, which is a general definition of activities and resources that usually yields several tasks\.

![\[AWS Data Pipeline task lifecycle\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-task-lifecycle.png)

There are two ways you can use Task Runner to process your pipeline: 
+  AWS Data Pipeline installs Task Runner for you on resources that are launched and managed by the AWS Data Pipeline web service\. 
+  You install Task Runner on a computational resource that you manage, such as a long\-running EC2 instance, or an on\-premises server\.

For more information about working with Task Runner, see [Working with Task Runner](dp-using-task-runner.md)\.