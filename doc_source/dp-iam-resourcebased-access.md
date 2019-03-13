# IAM Policies for AWS Data Pipeline<a name="dp-iam-resourcebased-access"></a>

By default, IAM users don't have permission to create or modify AWS resources\. To allow IAM users to create or modify resources and perform tasks, you must create IAM policies that grant IAM users permission to use the specific resources and API actions they'll need, and then attach those policies to the IAM users or groups that require those permissions\.

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\. For general information about IAM policies, see [Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide* guide\. For more information about managing and creating custom IAM policies, see [Managing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html)\.

**Topics**
+ [Policy Syntax](#dp-policy-syntax)
+ [Controlling Access to Pipelines Using Tags](#dp-control-access-tags)
+ [Controlling Access to Pipelines Using Worker Groups](#dp-control-access-workergroup)

## Policy Syntax<a name="dp-policy-syntax"></a>

An IAM policy is a JSON document that consists of one or more statements\. Each statement is structured as follows:

```
{
  "Statement":[{
    "Effect":"effect",
    "Action":"action",
    "Resource":"*",
    "Condition":{
      "condition":{
        "key":"value"
        }
      }
    }
  ]
}
```

The following elements make up a policy statement:
+ **Effect:** The *effect* can be `Allow` or `Deny`\. By default, IAM users don't have permission to use resources and API actions, so all requests are denied\. An explicit allow overrides the default\. An explicit deny overrides any allows\.
+ **Action**: The *action* is the specific API action for which you are granting or denying permission\. For a list of actions for AWS Data Pipeline, see [Actions](https://docs.aws.amazon.com/datapipeline/latest/APIReference/API_Operations.html) in the *AWS Data Pipeline API Reference*\.
+ **Resource**: The resource that's affected by the action\. The only valid value here is `"*"`\. 
+ **Condition**: Conditions are optional\. They can be used to control when your policy will be in effect\.

  AWS Data Pipeline implements the AWS\-wide context keys \(see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#AvailableKeys)\), plus the following service\-specific keys\.
  + `datapipeline:PipelineCreator` — To grant access to the user that created the pipeline\. For an example, see [Grant the pipeline owner full access](dp-example-tag-policies.md#ex3)\.
  + `datapipeline:Tag` — To grant access based on pipeline tagging\. For more information, see [Controlling Access to Pipelines Using Tags](#dp-control-access-tags)\.
  + `datapipeline:workerGroup` — To grant access based on the name of the worker group\. For more information, see [Controlling Access to Pipelines Using Worker Groups](#dp-control-access-workergroup)\.

## Controlling Access to Pipelines Using Tags<a name="dp-control-access-tags"></a>

You can create IAM policies that reference the tags for your pipeline\. This enables you to use pipeline tagging to do the following:
+ Grant read\-only access to a pipeline
+ Grant read/write access to a pipeline
+ Block access to a pipeline

For example, suppose that a manager has two pipeline environments, production and development, and an IAM group for each environment\. For pipelines in the production environment, the manager grants read/write access to users in the production IAM group, but grants read\-only access to users in the developer IAM group\. For pipelines in the development environment, the manager grants read/write access to both the production and developer IAM groups\.

To achieve this scenario, the manager tags the production pipelines with the "environment=production" tag and attaches the following policy to the developer IAM group\. The first statement grants read\-only access to all pipelines\. The second statement grants read/write access to pipelines that do not have an "environment=production" tag\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "datapipeline:Describe*",
        "datapipeline:ListPipelines",
        "datapipeline:GetPipelineDefinition",
        "datapipeline:QueryObjects"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "datapipeline:*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {"datapipeline:Tag/environment": "production"}
      }
    }
  ]
}
```

In addition, the manager attaches the following policy to the production IAM group\. This statement grants full access to all pipelines\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "datapipeline:*",
      "Resource": "*"
    }
  ]
}
```

For more examples, see [Grant users read\-only access based on a tag](dp-example-tag-policies.md#ex1) and [Grant users full access based on a tag](dp-example-tag-policies.md#ex2)\.

## Controlling Access to Pipelines Using Worker Groups<a name="dp-control-access-workergroup"></a>

You can create IAM policies that make reference worker group names\.

For example, suppose that a manager has two pipeline environments, production and development, and an IAM group for each environment\. The manager has three database servers with task runners configured for production, pre\-production, and developer environments, respectively\. The manager wants to ensure that users in the production IAM group can create pipelines that push tasks to production resources, and that users in the development IAM group can create pipelines that push tasks to both pre\-production and developer resources\.

To achieve this scenario, the manager installs task runner on the production resources with production credentials, and sets `workerGroup` to "prodresource"\. In addition, the manager installs task runner on the development resources with development credentials, and sets `workerGroup` to "pre\-production" and "development"\. The manager attaches the following policy to the developer IAM group to block access to "prodresource" resources\. The first statement grants read\-only access to all pipelines\. The second statement grants read/write access to pipelines when the name of the worker group has a prefix of "dev" or "pre\-prod"\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "datapipeline:Describe*",
        "datapipeline:ListPipelines",
        "datapipeline:GetPipelineDefinition",
        "datapipeline:QueryObjects"
      ],
      "Resource": "*"
    },
    {
      "Action": "datapipeline:*",
      "Effect": "Allow",
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "datapipeline:workerGroup": ["dev*","pre-prod*"]
        }
      }
    }
  ]
}
```

In addition, the manager attaches the following policy to the production IAM group to grant access to "prodresource" resources\. The first statement grants read\-only access to all pipelines\. The second statement grants read/write access when the name of the worker group has a prefix of "prod"\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "datapipeline:Describe*",
        "datapipeline:ListPipelines",
        "datapipeline:GetPipelineDefinition",
        "datapipeline:QueryObjects"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "datapipeline:*",
      "Resource": "*",
      "Condition": {
        "StringLike": {"datapipeline:workerGroup": "prodresource*"}
      }
    }
  ]
}
```