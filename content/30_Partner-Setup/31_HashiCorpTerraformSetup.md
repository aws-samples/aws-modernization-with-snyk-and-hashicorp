---
title: "HashiCorp Terraform Setup Instructions"
chapter: true
weight: 31 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# HashiCorp Terraform Setup Instructions 

Terraform is an open-source infrastructure as code software tool that enables you to safely and predictably create, change, and improve infrastructure Users define and provide data center infrastructure using a declarative configuration language known as HashiCorp Configuration Language, or optionally JSON.

This section covers installing the Terraform CLI and setting up Terraform Cloud Account.


## Terraform CLI

The Command-Line-Interface (CLI) to Terraform is via the terraform command, which accepts a variety of subcommands such as terraform init or terraform plan. A full list of all of the supported subcommands is by using terraform -help command.

Start by downloading the Terraform CLI to your environment. In this workshop, we’ll prescribe steps to save time and you can find more details on the Terraform learn site at:
https://learn.hashicorp.com/tutorials/terraform/install-cli

At the Cloud9 prompt, enter these commands to install the Terraform CLI using the yum package manager.

1. Install yum-config-manager to manage your repositories:

```
sudo yum install -y yum-utils
```

2. Next, Use yum-config-manager to add the official HashiCorp Linux repository:

```
sudo yum-config-manager --add-repo 'https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo'
```

3. Next, Install Terraform from the new repository.

```
sudo yum -y install terraform
```

4. Finally, verify successfull installation:

```
terraform -help
```

### Terraform Cloud

Terraform Cloud is an application that helps teams use Terraform together. It manages Terraform runs in a consistent and reliable environment, and includes easy access to shared state and secret data, access controls for approving changes to infrastructure, a private registry for sharing Terraform modules, detailed policy controls for governing the contents of Terraform configurations, and more.

You will be using Terraform Cloud to store the Terraform state of the infrastructures your pipeline will provision and deploy using Terraform in future modules.

### Create Terraform Cloud Access Token 

#### 1. Visit [Terraform Cloud ](https://app.terraform.io/signup/account) and follow the prompts to create a free Terraform Cloud account, if you already have an existing account [sign in](https://app.terraform.io/session). 

![TerraformCloudCreateAccount](/images/terraform-cloud-create-account.png)


When you sign up, you will receive an email asking you to confirm your email address. Confirm your email address before moving on. When you click the link to confirm your email address, the Terraform Cloud UI will ask which setup workflow you would like use. **Select Start from scratch**.

![TerraformCloudCreateOrgScratch](/images/terraform-cloud-create-org-scratch.png)

#### 2. Create a new [Terraform Cloud organization](https://learn.hashicorp.com/terraform/cloud-getting-started/signup#create-your-organization)

![TerraformCloudCreateOrg](/images/terraform-cloud-create-org.png)


#### 3. Create a new [Terraform Cloud workspace](https://learn.hashicorp.com/terraform/cloud-getting-started/create-workspace) with **CLI driven workflow**.

![TerraformCloudCreateWorkspace](/images/terraform-cloud-create-workspace.png)

In the workspace, set workspace name as **arm-aws-ecs**

![TerraformCloudWorkspaceConfig](/images/terraform-cloud-workspace-config.png)

#### 4. Go to the [User Settings section](https://app.terraform.io/app/settings/tokens) in the Terraform Cloud Dashboard and create a new API token.

![TerraformCloudUserSetting](/images/terraform-cloud-user-setting.png)

In the User Settings section, Create a new '[Terraform API token] (https://www.terraform.io/docs/cloud/users-teams-organizations/users.html#api-tokens)' then copy and paste the token in a secure location for later use.

{{% notice warning %}}
<p style='text-align: left;'>
Your Terraform Cloud API token must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
</p>
{{% /notice %}}

Great, you have created and safely stored your newly created Terraform Cloud API Token.