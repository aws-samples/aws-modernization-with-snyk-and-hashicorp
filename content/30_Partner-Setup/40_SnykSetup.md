---
title: "Snyk Setup Instructions"
chapter: true
weight: 40
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


### Docker Hub <!-- MODIFY THIS HEADING -->

Docker Hub is a service provided by Docker for finding and sharing container images with your team. It is the world’s largest repository of container images with an array of content sources including container community developers, open source projects and independent software vendors (ISV) building and distributing their code in containers.


### Create a Docker Hub Access Token <!-- MODIFY THIS HEADING -->

The pipeline will package the application into a Docker image. It then pushes that image a public Docker Hub image repository so that it will be available to the deployment segment of the pipeline. To push or upload the newly-built Docker image, the pipeline will need an access token to authorize transaction on your Docker Hub account. You will need to create a new access token (https://docs.docker.com/docker-hub/access-tokens/) and store it for use in later modules. To create your new access tokens:

    Log in to hub.docker.com
    Click your username in the top right corner and select Account Settings
    Select Security > New Access Token.
    Add a description for your token that indicates where the token will be used, or that sets a purpose for the token
    Copy the token that appears on the screen and record it in a safe location for use in future modules. Make sure you do this now! Once you close this prompt, Docker will never show the token again and you will have to create a new one


{{% notice warning %}}
<p style='text-align: left;'>
Docker Hub credentials and access tokens must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
</p>
{{% /notice %}}

Now that you have created and safely recorded your new access token, let’s move to the next section and create a new Snyk Access token.