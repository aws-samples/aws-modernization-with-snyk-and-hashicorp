---
title: "HashiCorp Terraform Cloud"
chapter: true
weight: 32
---

# HashiCorp Terraform Cloud Setup Instructions 

Terraform Cloud is an application that helps teams use Terraform together. It manages Terraform runs in a consistent and reliable environment, and includes easy access to shared state and secret data, access controls for approving changes to infrastructure, a private registry for sharing Terraform modules, detailed policy controls for governing the contents of Terraform configurations, and more.

You will be using Terraform Cloud to store the Terraform state of the infrastructures your pipeline will provision and deploy using Terraform in future modules.

## Create Terraform Cloud Access Token 

### 1. Visit [Terraform Cloud ](https://app.terraform.io/signup/account) and follow the prompts to create a free Terraform Cloud account, if you already have an existing account [sign in](https://app.terraform.io/session). 

![TerraformCloudCreateAccount](/images/terraform-cloud-create-account.png)


When you sign up, you will receive an email asking you to confirm your email address. Confirm your email address before moving on. When you click the link to confirm your email address, the Terraform Cloud UI will ask which setup workflow you would like use. **Select Start from scratch**.

![TerraformCloudCreateOrgScratch](/images/terraform-cloud-create-org-scratch.png)

### 2. Create a new [Terraform Cloud organization](https://learn.hashicorp.com/terraform/cloud-getting-started/signup#create-your-organization)

![TerraformCloudCreateOrg](/images/terraform-cloud-create-org.png)


### 3. Create a new [Terraform Cloud workspace](https://learn.hashicorp.com/terraform/cloud-getting-started/create-workspace) with **CLI driven workflow**.

![TerraformCloudCreateWorkspace](/images/terraform-cloud-create-workspace.png)

In the workspace, set workspace name as **arm-aws-ecs**

![TerraformCloudWorkspaceConfig](/images/terraform-cloud-workspace-config.png)

### 4. Go to the [User Settings section](https://app.terraform.io/app/settings/tokens) in the Terraform Cloud Dashboard and create a new API token.

![TerraformCloudUserSetting](/images/terraform-cloud-user-setting.png)

In the User Settings section, Create a new '[Terraform API token] (https://www.terraform.io/docs/cloud/users-teams-organizations/users.html#api-tokens)' then copy and paste the token in a secure location for later use.

{{% notice warning %}}
<p style='text-align: left;'>
Your Terraform Cloud API token must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
</p>
{{% /notice %}}

Great, you have created and safely stored your newly created Terraform Cloud API Token.