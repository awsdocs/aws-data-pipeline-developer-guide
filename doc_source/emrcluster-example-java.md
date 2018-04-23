# Use EmrCluster Resource in AWS SDK for Java<a name="emrcluster-example-java"></a>

**Example**  <a name="example7"></a>
The following example shows how to use an `EmrCluster` and `EmrActivity` to create an Amazon EMR 4\.x cluster to run a Spark step using the Java SDK:  

```
public class dataPipelineEmr4 {

  public static void main(String[] args) {
    
	AWSCredentials credentials = null;
	credentials = new ProfileCredentialsProvider("/path/to/AwsCredentials.properties","default").getCredentials();
	DataPipelineClient dp = new DataPipelineClient(credentials);
	CreatePipelineRequest createPipeline = new CreatePipelineRequest().withName("EMR4SDK").withUniqueId("unique");
	CreatePipelineResult createPipelineResult = dp.createPipeline(createPipeline);
	String pipelineId = createPipelineResult.getPipelineId();
    
	PipelineObject emrCluster = new PipelineObject()
	    .withName("EmrClusterObj")
	    .withId("EmrClusterObj")
	    .withFields(
			new Field().withKey("releaseLabel").withStringValue("emr-4.1.0"),
			new Field().withKey("coreInstanceCount").withStringValue("3"),
			new Field().withKey("applications").withStringValue("spark"),
			new Field().withKey("applications").withStringValue("Presto-Sandbox"),
			new Field().withKey("type").withStringValue("EmrCluster"),
			new Field().withKey("keyPair").withStringValue("myKeyName"),
			new Field().withKey("masterInstanceType").withStringValue("m3.xlarge"),
			new Field().withKey("coreInstanceType").withStringValue("m3.xlarge")        
			);
  
	PipelineObject emrActivity = new PipelineObject()
	    .withName("EmrActivityObj")
	    .withId("EmrActivityObj")
	    .withFields(
			new Field().withKey("step").withStringValue("command-runner.jar,spark-submit,--executor-memory,1g,--class,org.apache.spark.examples.SparkPi,/usr/lib/spark/lib/spark-examples.jar,10"),
			new Field().withKey("runsOn").withRefValue("EmrClusterObj"),
			new Field().withKey("type").withStringValue("EmrActivity")
			);
      
	PipelineObject schedule = new PipelineObject()
	    .withName("Every 15 Minutes")
	    .withId("DefaultSchedule")
	    .withFields(
			new Field().withKey("type").withStringValue("Schedule"),
			new Field().withKey("period").withStringValue("15 Minutes"),
			new Field().withKey("startAt").withStringValue("FIRST_ACTIVATION_DATE_TIME")
			);
      
	PipelineObject defaultObject = new PipelineObject()
	    .withName("Default")
	    .withId("Default")
	    .withFields(
			new Field().withKey("failureAndRerunMode").withStringValue("CASCADE"),
			new Field().withKey("schedule").withRefValue("DefaultSchedule"),
			new Field().withKey("resourceRole").withStringValue("DataPipelineDefaultResourceRole"),
			new Field().withKey("role").withStringValue("DataPipelineDefaultRole"),
			new Field().withKey("pipelineLogUri").withStringValue("s3://myLogUri"),
			new Field().withKey("scheduleType").withStringValue("cron")
			);     
      
	List<PipelineObject> pipelineObjects = new ArrayList<PipelineObject>();
    
	pipelineObjects.add(emrActivity);
	pipelineObjects.add(emrCluster);
	pipelineObjects.add(defaultObject);
	pipelineObjects.add(schedule);
    
	PutPipelineDefinitionRequest putPipelineDefintion = new PutPipelineDefinitionRequest()
	    .withPipelineId(pipelineId)
	    .withPipelineObjects(pipelineObjects);
    
	PutPipelineDefinitionResult putPipelineResult = dp.putPipelineDefinition(putPipelineDefintion);
	System.out.println(putPipelineResult);
    
	ActivatePipelineRequest activatePipelineReq = new ActivatePipelineRequest()
	    .withPipelineId(pipelineId);
	ActivatePipelineResult activatePipelineRes = dp.activatePipeline(activatePipelineReq);
	
      System.out.println(activatePipelineRes);
      System.out.println(pipelineId);
    
    }

}
```