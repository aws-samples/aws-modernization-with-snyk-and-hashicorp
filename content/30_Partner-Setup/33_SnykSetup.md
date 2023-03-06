---
title: "Snyk CLI"
chapter: true
weight: 33
---

# Snyk Setup Instructions
You will need a Snyk account to run scans.  Snyk is available for free and all you need is a valid email address to register.  Once you register, you can perform scans and view results locally or on the website.

## Setting up your Snyk Account

### I do not have a Snyk account
[You can register for a FREE account here.](https://app.snyk.io/signup/?utm_medium=Partner&utm_source=Atlassian&utm_campaign=Bitbucket-cloud-promo-Q1-2020)

### I already have a Snyk account
[Log in to your account here.](https://app.snyk.io/signup/?utm_medium=Partner&utm_source=Atlassian&utm_campaign=Bitbucket-cloud-promo-Q1-2020)

### Create Snyk Access Token
- Visit your Snyk account (Account Settings > API Token section) (https://app.snyk.io/account)
- In the KEY field, select click to show, then select and copy your API token from the field
- Paste the token that appears on the screen in a safe location for use in future modules

{{% notice warning %}}
<p style='text-align: left;'>
Your Snyk access token must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
</p>
{{% /notice %}}

You can read more about Snyk Access Token from their docs here.

## Setting up the Snyk CLI

The Snyk Command-Line-Interface (CLI) is highly portable and very popular with end users.  We’ll use the Snyk CLI in this workshop to collect and send results about your vulnerabilities.

Start by downloading the Snyk CLI to your environment.  In this workshop, we’ll prescribe steps to save time and you can find more details on the Snyk documentation site at:
https://docs.snyk.io/snyk-cli/install-the-snyk-cli

At the Cloud9 prompt, enter these commands to download the binary for Linux and move them to your bin folder (/usr/local/bin):

```
curl https://static.snyk.io/cli/latest/snyk-linux -o snyk && \
chmod +x ./snyk && \
sudo mv ./snyk /usr/local/bin/
```

In Cloud9 environments, you will need to authenticate on the CLI with your API token.  Previously, you should have created an API token.  If not, navigate to your Snyk Account (https://app.snyk.io/account), and get your API_TOKEN by clicking into your Account Settings -> API Token section.

In the KEY field, click your “click to show” box to copy your API token.

You can then run this command where API_TOKEN is the value you copied.

```
snyk auth API_TOKEN.
```

That should be it!  Your response should look like the following:

    snyk auth 12345678-abcd-efgh-1234head5678bead

    Your account has been authenticated. Snyk is now ready to be used.

If you are not on a Cloud9 environment, then your CLI should be able to start up a web browser and you can authenticate with this command:

```
snyk auth
```

In this case, you'll authenticate by logging on with the web UI of Snyk.


### Next Section: Running with Snyk
Great, you have created and safely stored your newly created Snyk access token, Now, let’s create the Terraform Cloud access token.
