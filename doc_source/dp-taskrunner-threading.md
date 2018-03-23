# Task Runner Threads and Preconditions<a name="dp-taskrunner-threading"></a>

 Task Runner uses a thread pool for each of tasks, activities, and preconditions\. The default setting for `--tasks` is 2, meaning that there are two threads allocated from the tasks pool and each thread polls the AWS Data Pipeline service for new tasks\. Thus, `--tasks` is a performance tuning attribute that can be used to help optimize pipeline throughput\.

 Pipeline retry logic for preconditions happens in Task Runner\. Two precondition threads are allocated to poll AWS Data Pipeline for precondition objects\. Task Runner honors the precondition object **retryDelay** and **preconditionTimeout** fields that you define on preconditions\. 

In many cases, decreasing the precondition polling timeout and number of retries helps to improve the performance of your application\. Similarly, applications with long\-running preconditions may need to have the timeout and retry values increased\. For more information about precondition objects, see [Preconditions](dp-concepts-preconditions.md)\.