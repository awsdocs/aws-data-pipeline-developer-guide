# Step 5: Verify the Data Import<a name="dp-import-ddb-pipelinejson-verifydata"></a>

Next, verify that the data import occurred successfully using the DynamoDB console to inspect the data in the table\.

**To verify the DynamoDB table**

1. Open the DynamoDB console\.

1. On the **Tables** screen, click your DynamoDB table and click **Explore Table**\.

1. On the **Browse Items** tab, columns that correspond to the data input file should display, such as Id, Price, ProductCategory, as shown in the following screen\. This indicates that the import operation from the file to the DynamoDB table occurred successfully\.  
![\[Browse items tab\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-ddb-verify-importdata.png)