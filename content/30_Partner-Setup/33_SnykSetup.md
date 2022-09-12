---
title: "Snyk CLI"
chapter: true
weight: 33
---

# Snyk Setup Instructions
Snyk is an open source security platform designed to help software-driven businesses enhance developer security. Snyk’s dependency scanner makes it the only solution that seamlessly and proactively finds, prioritizes, and fixes vulnerabilities and license violations in open source dependencies and container images.

## Create Snyk Access Token

    Visit your Snyk account (Account Settings > API Token section) (https://app.snyk.io/account)
    In the KEY field, select click to show, then select and copy your API token from the field
    Paste the token that appears on the screen in a safe location for use in future modules

{{% notice warning %}}
<p style='text-align: left;'>
Your Snyk access token must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
</p>
{{% /notice %}}

You can read more about Snyk Access Token from their docs here.

Great, you have created and safely stored your newly created Snyk access token, Now, let’s create the Terraform Cloud access token.


## Snyk CLI

The Snyk Command-Line-Interface (CLI) is highly portable and very popular with end users.  We’ll use the Snyk CLI in this workshop to collect and send results about your vulnerabilities.

Start by downloading the Snyk CLI to your environment.  In this workshop, we’ll prescribe steps to save time and you can find more details on the Snyk documentation site at:
https://docs.snyk.io/snyk-cli/install-the-snyk-cli

At the Cloud9 prompt, enter these commands to download the binary for Linux and move them to your bin folder (/usr/local/bin):

```
curl https://static.snyk.io/cli/latest/snyk-linux -o snyk
chmod +x ./snyk
mv ./snyk /usr/local/bin/
```

Next, authenticate with Snyk by typing in the command below:

```
snyk auth
```

When you run that command, you’ll be directed to authenticate in a browser.  If your environment does not bring up a web browser, you can instead get your authorization token from the Snyk UI.
You get your API_TOKEN by clicking into your Snyk Account (https://app.snyk.io/account), by clicking through the Account Settings -> API Token section.
In the KEY field, click your “click to show” box to copy your API token.
Finally, run this command where API_TOKEN is the value you copied.

```
snyk auth API_TOKEN.
```

That should be it!  Your response should look like the following:

**TODO: Show a screenshot of the operation**

### Next Section: Running with Snyk
Now that you have setup Snyk and the CLI, you are ready to start your workshop.
