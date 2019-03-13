# RdsDatabase<a name="dp-object-rdsdatabase"></a>

Defines an Amazon RDS database\.

**Note**  
RdsDatabase does not support Aurora\. Use [JdbcDatabase](dp-object-jdbcdatabase.md) for Aurora, instead\.

## Example<a name="rdsdatabase-example"></a>

The following is an example of this object type\.

```
{
  "id" : "MyRdsDatabase",
  "type" : "RdsDatabase",
  "region" : "us-east-1",
  "username" : "user_name",
  "*password" : "my_password",
  "rdsInstanceId" : "my_db_instance_identifier"
}
```

For the Oracle engine, the `jdbcDriverJarUri` field is required and you can specify the following driver: `http://www.oracle.com/technetwork/database/features/jdbc/jdbc-drivers-12c-download-1958347.html`\. For the SQL Server engine, the `jdbcDriverJarUri` field is required and you can specify the following driver: `https://www.microsoft.com/en-us/download/details.aspx?displaylang=en&id=11774`\. For the MySQL and PostgreSQL engines, the `jdbcDriverJarUri` field is optional\.

## Syntax<a name="rdsdatabase-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| \*password | The password to supply\. | String | 
| rdsInstanceId | The identifier of the DB instance\. | String | 
| username | The user name to supply when connecting to the database\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| databaseName | Name of the logical database to attach to | String | 
| jdbcDriverJarUri | The location in Amazon S3 of the JDBC driver JAR file used to connect to the database\. AWS Data Pipeline must have permission to read this JAR file\. For the MySQL and PostgreSQL engines, the default driver is used if this field is not specified, but you can override the default using this field\. For the Oracle and SQL Server engines, this field is required\. | String | 
| jdbcProperties | Pairs of the form A=B that will be set as properties on JDBC connections for this database\. | String | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, for example, "parent":\{"ref":"myBaseObjectId"\} | 
| region | The code for the region where the database exists\. For example, us\-east\-1\. | String | 


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