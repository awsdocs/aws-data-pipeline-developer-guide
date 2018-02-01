# Cloning Your Pipeline<a name="dp-manage-pipeline-clone-console"></a>

Cloning makes a copy of a pipeline and allows you to specify a name for the new pipeline\. You can clone a pipeline that is in any state, even if it has errors; however, the new pipeline remains in the `PENDING` state until you manually activate it\. For the new pipeline, the clone operation uses the latest version of the original pipeline definition rather than the active version\. In the clone operation, the full schedule from the original pipeline is not copied into the new pipeline, only the period setting\.

**Note**  
You can't clone a pipeline using the command line interface \(CLI\)\.

**To clone a pipeline using the console**

1. In the **List Pipelines** page, select the pipeline to clone\.

1. Click **Actions**, and then click **Clone**\.

1. In the **Clone a Pipeline** dialog box, enter a name for the new pipeline and click **Clone**\.

1. In the **Schedule** pane, specify a schedule for the new pipeline\.

1. To activate the new pipeline, click **Actions**, and then click **Activate**\.