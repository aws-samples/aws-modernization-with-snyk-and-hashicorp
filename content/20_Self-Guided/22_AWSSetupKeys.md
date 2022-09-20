---
title: "Setting up your AWS keys"
chapter: true
weight: 23 
---

# Setting up your access keys

You will need to setup AWS access keys to enable Terraform Cloud to deploy to your AWS instance.

{{% notice warning %}}
Treat your AWS Keys as valuable secrets.  If these keys are revealed online via a screenshare, git check-in, or other means, your AWS account will be at risk.  Do not share your keys with anyone.
{{% /notice %}}

Log onto AWS and navigate to your [IAM user account](https://console.aws.amazon.com/iam/home?#/users)

On that screen find your name.  You may also search for your name to narrow down the listing.  Click on your name.

![aws-add-user-1](/images/aws-iam-user-1.png)

Click on the **Security credentials** tab for your user.

![aws-add-user-1](/images/aws-iam-user-2.png)

Find the button to **Create access key** and press it.
![aws-add-user-1](/images/aws-iam-user-3.png)

The next screen is specific to you and contains your secret **ACCESS_KEY_ID** and **SECRET_ACCESS_KEY**.  Copy those values now because this is the only time you will see them onscreen.
Alternatively, you can download the *.csv* file with the contents and save them somewhere secure.
![aws-add-user-1](/images/aws-iam-user-4.png)

### Next Section: Create a Workspace
In the next section, you will create create an AWS Cloud9 workspace for your operations.

