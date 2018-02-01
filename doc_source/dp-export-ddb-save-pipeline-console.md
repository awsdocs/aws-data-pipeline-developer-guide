# Step 2: Save and Validate Your Pipeline<a name="dp-export-ddb-save-pipeline-console"></a>

You can save your pipeline definition at any point during the creation process\. As soon as you save your pipeline definition, AWS Data Pipeline looks for syntax errors and missing values in your pipeline definition\. If your pipeline is incomplete or incorrect, AWS Data Pipeline generates validation errors and warnings\. Warning messages are informational only, but you must fix any error messages before you can activate your pipeline\.

**To save and validate your pipeline**

1. Choose **Save pipeline**\.

1. AWS Data Pipeline validates your pipeline definition and returns either success or error or warning messages\. If you get an error message, choose **Close** and then, in the right pane, choose **Errors/Warnings**\.

1. The **Errors/Warnings** pane lists the objects that failed validation\. Choose the plus \(**\+**\) sign next to the object names and look for an error message in red\.

1. When you see an error message, go to the specific object pane where you see the error and fix it\. For example, if you see an error message in the **DataNodes** object, go to the **DataNodes** pane to fix the error\.

1. After you fix the errors listed in the **Errors/Warnings** pane, choose **Save Pipeline**\.

1. Repeat the process until your pipeline validates successfully\.