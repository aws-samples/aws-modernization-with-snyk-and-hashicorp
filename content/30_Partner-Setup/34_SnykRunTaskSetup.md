---
title: "Snyk Run Task"
chapter: true
weight: 34
---

# Snyk Terraform Cloud Run Task Instructions
Snyk is available in Terraform Cloud as a Run Task to leverage Snyk results in your workflow.

{{% notice info %}}
<p style='text-align: left;'>
Your Terraform Cloud configuration must support Run Tasks.  This feature is enabled in Terraform Cloud Business Plans and higher.
</p>
{{% /notice %}}

## 1. Sign In to Terraform Cloud


## 2. Navigate into your General Settings, and Run tasks
Navigate into your workspace and click on your Settings page, and then the settings page for your workspace.  This opens the settings for your entire workspace.
![TerraformCloudSettings](/images/tfc-toolbar-1.png)

From the settings page navigate into your Run Tasks section from the left-side of your Integrations page.
![TerraformCloudSettings](/images/tfc-settings-1.png)

You'll next see a button to **Create run task** which we'll click.
![TerraformCloudSettings](/images/tfc-settings-run-tasks-1.png)

You will need four pieces of information, and two come from the Snyk website.  Two are values you enter, the the other two are from the Snyk website.
![TerraformCloudSettings](/images/tfc-settings-run-tasks-2.png)

At this point, you can fill in two fields.

1. Name your run task something suitable such as `snyk-run-task-for-workshop`.
1. Enter a description suitable for this workshop or leave the field blank.  


Let's next navigate to Snyk.io to get the remaining fields.  Keep this page open.

## Navigate to Snyk.io
Navigate to your snyk.io account and click on the integrations tab of your toolbar.
![SnykSettings](/images/snyk-settings-toolbar-2.png)

Search for or find the tile for Terraform Cloud.
![SnykTerraformCloud1](/images/snyk-tfc-settings-1.png)

Within the Terraform Cloud configuration page, identify the fields for your URL and  HMAC key.  These are two values you'll copy and enter into the Terraform Cloud page for their respective values.
![SnykTerraformCloud2](/images/snyk-tfc-settings-2.png)

When you are finished, your screen will look similar to what is shown below with your details.  Click on the enabled, **Create run task** button to finish.  This process validates your URL and HMAC key.
![SnykTerraformCloud3](/images/tfc-settings-run-tasks-3.png)

When you are finished, your will see your new run tasks added to a list of available tasks.  You are ready to use the Snyk Run Task in Terraform Cloud.

### Next Section: Running the Workshop
Now that you have setup Snyk and the CLI, you are ready to start your workshop.
