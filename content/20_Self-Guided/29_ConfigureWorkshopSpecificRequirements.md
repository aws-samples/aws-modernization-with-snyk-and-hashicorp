---
title: "Configure workshop specific requirements"
chapter: true
weight: 29
---

# Configuring Workshop Specific Requirements
# Configure Workspace

Return to your workspace and click the gear icon (in top right corner), or click to open a new tab and choose “Open Preferences”

1. Select AWS SETTINGS and turn off AWS managed temporary credentials
1. Close the Preferences tab
1. Turn off temp credentials

## Update awscli

TODO: Verify if we even need this operation.  We may ask people ton figure their AWS access keys.
Next, ensure your version of the AWS CLI is up to date.


```
sudo pip install --upgrade awscli && hash -r
```

## Create EC2 key pair

Search for Key Pair on the AWS portal search bar, click on Key pairs under EC2 feature

![AWSKeyPair1](/images/aws-key-pair-1.png)

Hit Create key pair

![AWSKeyPair2](/images/aws-key-pair-2.png)

Hit Create key pair again, this will also download a aws-workshop.pem file which we'll later use to SSH into EC2 machines

![AWSKeyPair3](/images/aws-key-pair-3.png)


## Next Section: Partner Setup
In the next section we configure partner tooling.
