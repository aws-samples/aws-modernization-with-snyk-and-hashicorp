---
title: "HashiCorp HCP Terraform"
chapter: true
weight: 32
---

# HashiCorp HCP Terraform Setup Instructions 

HCP Terraform is an application that helps teams use Terraform together. It manages Terraform runs in a consistent and reliable environment, and includes easy access to shared state and secret data, access controls for approving changes to infrastructure, a private registry for sharing Terraform modules, detailed policy controls for governing the contents of Terraform configurations, and more.

You will be using HCP Terraform to store the Terraform state of the infrastructures your pipeline will provision and deploy using Terraform in future modules.

## Create HCP Terraform Access Token 

### 1. Visit [HCP Terraform ](https://app.terraform.io/signup/account) and follow the prompts to create a free HCP Terraform account, if you already have an existing account [sign in](https://app.terraform.io/session). 

![TerraformCloudCreateAccount](/images/terraform-cloud-create-account.png)


When you sign up, you will receive an email asking you to confirm your email address. Confirm your email address before moving on. When you click the link to confirm your email address, the HCP Terraform UI will ask which setup workflow you would like use. **Select Start from scratch**.

![TerraformCloudCreateOrgScratch](/images/terraform-cloud-create-org-scratch.png)

### 2. Create a new [HCP Terraform organization](https://app.terraform.io/app/organizations/new)

Create a new HCP Terraform organization using this link:

https://app.terraform.io/app/organizations/new


![TerraformCloudCreateOrg](/images/terraform-cloud-create-org.png)




### 3. Create a new [HCP Terraform workspace](https://learn.hashicorp.com/terraform/cloud-getting-started/create-workspace) with CLI driven workflow.

![TerraformCloudCreateWorkspace](/images/terraform-cloud-create-workspace.png)

In the workspace, set workspace name as **aws-workshop**

![TerraformCloudWorkspaceConfig](/images/terraform-cloud-workspace-config.png)

### 4. Go to the [User Settings section](https://app.terraform.io/app/settings/tokens) in the HCP Terraform Dashboard and create a new API token.

![TerraformCloudUserSetting](/images/terraform-cloud-user-setting.png)

In the User Settings section, Create a new '[Terraform API token] (https://www.terraform.io/docs/cloud/users-teams-organizations/users.html#api-tokens)' then copy and paste the token in a secure location for later use.

{{% notice warning %}}
<p style='text-align: left;'>
Your HCP Terraform API token must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
</p>
{{% /notice %}}

Great, you have created and safely stored your newly created HCP Terraform API Token.
