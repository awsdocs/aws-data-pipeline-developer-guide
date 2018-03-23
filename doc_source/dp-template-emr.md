# Run Job on an Amazon EMR Cluster<a name="dp-template-emr"></a>

The **Run Job on an Elastic MapReduce Cluster** template launches an Amazon EMR cluster based on the parameters provided and starts running steps based on the specified schedule\. Once the job completes, the EMR cluster is terminated\. Optional bootstrap actions can be specified to install additional software or to change application configuration on the cluster\.

The template uses the following pipeline objects:
+ [EmrActivity](dp-object-emractivity.md)
+ [EmrCluster](dp-object-emrcluster.md)