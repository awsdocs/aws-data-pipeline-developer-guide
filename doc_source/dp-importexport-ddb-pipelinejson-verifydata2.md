# Step 5: Verify the Data Export File<a name="dp-importexport-ddb-pipelinejson-verifydata2"></a>

Next, verify that the data export occurred successfully using viewing the output file contents\.

**To view the export file contents**

1. Open the Amazon S3 console\.

1. On the **Buckets** pane, click the Amazon S3 bucket that contains your file output \(the example pipeline uses the output path **s3://mybucket/output/MyTable**\) and open the output file with your preferred text editor\. The output file name is an identifier value with no extension, such as this example: `ae10f955-fb2f-4790-9b11-fbfea01a871e_000000`\.

1. Using your preferred text editor, view the contents of the output file and ensure that there is a data file that corresponds to the DynamoDB source table\. The presence of this text file indicates that the export operation from DynamoDB to the output file occurred successfully\. 