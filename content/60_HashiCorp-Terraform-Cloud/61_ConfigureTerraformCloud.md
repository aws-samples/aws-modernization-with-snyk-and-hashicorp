---
title: "Configure Terraform Cloud"
chapter: true
weight: 61
---

## Create a variable set in Terraform Cloud

In order for Terraform Cloud to create resources for you in AWS it needs access keys, we can provide this key using variable sets.

First step is to open settings

![TerraformCloudSettings](/images/tfc-settings-1.png)

Next we'll click on **Variable sets** and create a new variable set

![TerraformCloudSettings1](/images/tfc-settings-var-sets-1.png)

We'll need to provide 3 AWS variables

* AWS_ACCESS_KEY_ID
* AWS_SECRET_ACCESS_KEY
* AWS_SESSION_TOKEN

Make sure these values are environment variables & the settings box on the far right is checked, click **Add variable** when finished adding one variable to add the next one

![TerraformCloudSettings2](/images/tfc-settings-var-sets-2.png)

Upon completion Variable sets should look like this, hit Create **variable set**.

![TerraformCloudSettings3](/images/tfc-settings-var-sets-3.png)

## Attach the Snyk run task to a workspace

Let's attach the Snyk run task we created in the Partner Setup module to a workspace.

We'll start by going in your workspace, click on **aws-workshop** workspace, open the settings menu and select Run Tasks on the left hand menu

Select the Snyk workshop run task created earlier in the workshop

![TerraformCloudWorkspaceRunTask1](/images/tfc-workspace-run-task-1.png)

Make sure that it's set to the **post-plan** option and the Enforcement level is set to **Manditory**, hit create. 

![TerraformCloudWorkspaceRunTask2](/images/tfc-workspace-run-task-2.png)

Great! In the next section we'll start working with Terraform Cloud workflows!