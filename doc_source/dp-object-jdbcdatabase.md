# JdbcDatabase<a name="dp-object-jdbcdatabase"></a>

Defines a JDBC database\.

## Example<a name="jdbcdatabase-example"></a>

The following is an example of this object type\.

```
{
  "id" : "MyJdbcDatabase",
  "type" : "JdbcDatabase",
  "connectionString" : "jdbc:redshift://hostname:portnumber/dbname",
  "jdbcDriverClass" : "com.amazon.redshift.jdbc41.Driver",
  "jdbcDriverJarUri" : "s3://redshift-downloads/drivers/RedshiftJDBC41-1.1.6.1006.jar",
  "username" : "user_name",
  "*password" : "my_password"
}
```

## Syntax<a name="jdbcdatabase-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| connectionString | The JDBC connection string to access the database\. | String | 
| jdbcDriverClass | The driver class to load before establishing the JDBC connection\. | String | 
| \*password | The password to supply\. | String | 
| username | The user name to supply when connecting to the database\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| databaseName | Name of the logical database to attach to | String | 
| jdbcDriverJarUri | The location in Amazon S3 of the JDBC driver JAR file used to connect to the database\. AWS Data Pipeline must have permission to read this JAR file\. | String | 
| jdbcProperties | Pairs of the form A=B that will be set as properties on JDBC connections for this database\. | String | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @version | Pipeline version that the object was created with\. | String | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | ID of the pipeline to which this object belongs\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects\. | String | 