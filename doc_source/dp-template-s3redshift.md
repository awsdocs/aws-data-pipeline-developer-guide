# Load Data from Amazon S3 into Amazon Redshift<a name="dp-template-s3redshift"></a>

The **Load data from S3 into Redshift** template copies data from an Amazon S3 folder into an Amazon Redshift table\. You can load the data into an existing table or provide a SQL query to create the table\. 

The data is copied based on the Amazon Redshift `COPY` options\. The Amazon Redshift table must have the same schema as the data in Amazon S3\. For `COPY` options, see [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) in the Amazon Redshift *Database Developer Guide*\. 

The template uses the following pipeline objects:
+ [CopyActivity](dp-object-copyactivity.md)
+ [RedshiftCopyActivity](dp-object-redshiftcopyactivity.md)
+ [S3DataNode](dp-object-s3datanode.md)
+ [SqlDataNode](dp-object-sqldatanode.md)
+ [RedshiftDataNode](dp-object-redshiftdatanode.md)
+ [RedshiftDatabase](dp-object-redshiftdatabase.md)
+ [Ec2Resource](dp-object-ec2resource.md)