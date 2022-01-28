---
title: Part 1  - Initial Workshop Setup  
description: Learn Rafay Kubernetes Operation Platform (KOP) with EKS Workshop. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability.
chapter: true
weight: 11
---


## What Will You Do

This is part 1 of a multi-part workshop. In this section, you will perform a few "one-time" tasks required to connect a Rafay account to an AWS account along with additional setup of tools that will be used throughout the workshop. 

---

## Step 1: Create Cloud Credentials 

Cloud credentials provide privileges to programmatically interact with your Amazon AWS account so that the lifecycle of infrastructure associated with the Amazon EKS cluster can be managed by Rafay's Kubernetes Operations Platform. 

- Login to the Rafay Console with the credentials provided and click on Infrastructure

- Select "Cloud Credentials", Click on "New Credential" and provide a unique name "aws-workshop-x" where x aligns with your Rafay user.

![Create Cloud Credential](/040_modules/img/part1/cloud_credential_create.png)

- Click on the drop down for "Credential Type" and select Role 

- Click the Copy button for the "External ID"

- Click on the following CloudFormation Template link and login to the AWS console with the credentials you have been provided:  https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=Rafay-Cloud-Credential&templateURL=https://rafay-aws-workshop.s3.us-west-2.amazonaws.com/Rafay-Cloud-Credential.template

- Once logged in to the AWS console you should be at the "Create Stack" page. Click Next

- This brings up the "Specify Stack Details" page -> Enter the External ID copied from Rafay Credential Screen above into the "ExternalID" field and click next

- This brings up the "Configure Stack Options" page -> Click Next

- This brings up the "Review Rafay-Cloud-Credential" page.  Scroll to the bottom, select that you acknowledge that the CloudFormation template might create an IAM resource and then click "Create Stack". 

- Wait 10 seconds and refresh the webpage.  Go to "Outputs" Tab at the top of the page and copy the Role ARN value

- Return to the Rafay Console and paste the Role ARN value into Role ARN section in "Add Credential"

![Create Cloud Credential](/040_modules/img/part1/cloud_credential_create.png)

- Write down the name created above "aws-workshop-x" for the cloud credential.  Note: The specification files will need to be updated to match this unique name.

---

## Step 2: Download RCTL

The RCTL CLI allows you to programmatically interact with the controller enabling users to construct sophisticated automation workflows. 

Run the following commands in the Cloud9 instance to download and extract RCTL

```
curl -s -o rctl-linux-amd64.tar.bz2 https://s3-us-west-2.amazonaws.com/rafay-prod-cli/publish/rctl-linux-amd64.tar.bz2
tar -xf rctl-linux-amd64.tar.bz2
chmod 0755 rctl
```

---

## Set Path for RCTL 

After downloading the RCTL CLI, run the command below to add it to your OS's PATH environment variable. 

```
export PATH=$PATH:/home/ec2-user/environment
```

## Initialize RCTL
  
The RCTL utility needs to be initialized with credentials and other information before it can interact with the Controller. At a given time, RCTL can be initialized with only one configuration. It can always be reinitialized if it needs to be bound to a different Org/Tenant.

Note: The RBAC associated with the user's credentials is automatically enforced.

RCTL supports both a "config file" as well as "dynamic config" model. The latter is well suited for automation pipelines where the configuration is provided dynamically and there is no need to permanently bind RCTL to an Org or Project. For today's workshop, we will use a "config file".

### RCTL Config File
- Navigate to the "My Tools" page in the Rafay Web Console
- Click on __Download CLI Config__ to download the configuration file.
- Save the configuration file on your local system

![CLI Tools Page](/040_modules/img/part1/cli_tools_page.png)

- In the Cloud9 interface, go to File -> Upload Local Files...
- Drag and drop or select the CLI config file that was previously downloaded
- In the Cloud9 terminal, run the following command to initilize RCTL

```
rctl config init <CONFIG FILE NAME>
```
## View RCTL Config

You can view the current configuration for RCTL by using the "config show" command.

```
rctl config show

Profile:                                                                    prod
REST Endpoint:                                                 console.rafay.dev
OPS Endpoint:                                                      ops.rafay.dev
API Key:                                                            <Masked>
API Secret:                                                         <Masked>
Project:                                                          defaultproject
```

---

## Projects

Projects are a way to implement multi tenancy within an Organization and implement true isolation boundaries. 

!!! Note
    Creation and Deletion of Projects are privileged operations. This is typically performed by an Org Admin using the Web Console because RBAC assignments also need to be implemented along with this.

---

### Default Project

By default, the RCTL config points to the "defaultproject". You can verify this by viewing the config.

```
rctl config show
```

---

### Set Project

For this workshop each person will have their own project that will be provided along with the credentials. This step will ensure that you set the project context before you can perform operations in this project. For example, to set the project context to your Rafay project provided in the workshop run the below command with the name of the project provided

```
rctl config set project <NAME OF ASSIGNED PROJECT>
```

Once the project context has been successfully set, ensure you verify this in your local config file.


!!! Note
    The name of the project is case-sensitive

---

## Step 3: Clone Git Repo 

Declarative specs for the Amazon EKS cluster and other resources are available in a [Git repository](https://github.com/RafaySystems/getstarted).  A GitHub account is required for this step.  If you don't have a GitHub account, you can sign-up here:  https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home

- Clone the Git repository to your laptop using the command below. 

```
git clone https://github.com/RafaySystems/getstarted.git
```

- Once complete, you should see a folder called "cloudwatch" which contains the specs needed for this guide. 

--- 

## Recap

At this point, you have everything setup and configured for the workshop.

---
