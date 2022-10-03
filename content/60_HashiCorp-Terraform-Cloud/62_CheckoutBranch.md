---
title: "Checkout branch from the repository"
chapter: true
weight: 62
---

## Checkout branch from the repository

Let's start by checking out an existing branch of the repository you've been working with so far.  In your working environment, run these commands to start from your home directory and checkout the branch named 'tfc'.

You should have some local changes, we'll stash them before proceeding.

```bash
cd ~/environment/vulnerable-ec2
git stash
git checkout tfc
```

Your Terraform Cloud workspace should look something like this in the UI, follow the instructions below

![TerraformCloudWorkspaceConfig](/images/terraform-cloud-configure-workspace.png)


Setup your Terraform Cloud account in the CLI, refer to Partner Setup Module > HashiCorp Terraform Cloud section

```bash
export TF_TOKEN_app_terraform_io="<your_token_here>"
```

Configure your Terraform Cloud org & workspace, refer Partner Setup section for more context. Change the feilds ""

```bash
vi main.tf
```
```sh

# add org & workspace names below 
terraform {

 cloud {
    organization = "organization-name-here"

    workspaces {
      name = "workspace-name-here"
    }
  }

.
.
.
```
