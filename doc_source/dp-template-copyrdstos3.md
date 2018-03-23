# Full Copy of Amazon RDS MySQL Table to Amazon S3<a name="dp-template-copyrdstos3"></a>

The **Full Copy of RDS MySQL Table to S3** template copies an entire Amazon RDS MySQL table and stores the output in an Amazon S3 location\. The output is stored as a CSV file in a timestamped subfolder under the specified Amazon S3 location\. 

The template uses the following pipeline objects:
+ [CopyActivity](dp-object-copyactivity.md)
+ [Ec2Resource](dp-object-ec2resource.md)
+ [SqlDataNode](dp-object-sqldatanode.md)
+ [S3DataNode](dp-object-s3datanode.md)