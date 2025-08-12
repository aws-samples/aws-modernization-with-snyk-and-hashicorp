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

Great! In the next section we'll start working with Terraform Cloud workflows!