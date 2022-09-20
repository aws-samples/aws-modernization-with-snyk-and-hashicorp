---
title: "Setting up your AWS keys"
chapter: true
weight: 22 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Setting up your AWS account <!-- MODIFY THIS HEADING -->

If you don’t already have an AWS account with Administrator access: create one now by clicking <a href="https://aws.amazon.com/getting-started/">here</a>.

Once you have an AWS account, ensure you are following the remaining workshop steps as an IAM user with administrator access to the AWS account: <a href="https://console.aws.amazon.com/iam/home?#/users$new">Create a new IAM user to use for the workshop</a>

Enter the user details: create user
![aws-add-user-1](/images/aws-add-user-1.png)

Attach the AdministratorAccess IAM Policy: attach policy
![aws-add-user-2](/images/aws-add-user-2.png)

Skip the part to add tags.  Click to create the new user: finish creation
![aws-add-user-3](/images/aws-add-user-3.png)

Take note of the login URL and save: login url
![aws-add-user-4](/images/aws-add-user-4.png)

### Next Section: Creating AWS Keys
In the next sectio, you will create AWS keys to allow Terraform to deply to AWS on your behalf.

