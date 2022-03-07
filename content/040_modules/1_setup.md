---
title: Part 1  - Rafay account setup  
description: Learn Rafay Kubernetes Operation Platform (KOP) with EKS Workshop. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability.
chapter: true
weight: 11
---

In this section, you will perform a few one-time tasks required to connect a Rafay account to an AWS account along with additional setup of tools that will be used throughout the workshop. We also will learn some key concepts used during the configuration of Rafay account.

---

## Multi-tenancy
Every enterprise has multiple teams, business units and sometimes even multiple production environments. Single cluster, Kubernetes cluster level multi tenancy using namespaces does not work for multi cluster and complex organizations.

### Organizations
Enterprises can optionally use completely separate Orgs (aka tenants) ensuring complete isolation. Authorized users can seamlessly switch between different Organizations with the click of a button. An Org can host multiple projects.
In this workshop all pre-configured Rafay accounts belong to **AWS Organization** created for training purposes only.

### Projects

Projects are a way to implement multi tenancy within an Organization and implement true isolation boundaries across "different operating environments", "different business units" etc. A project can host multiple Kubernetes clusters.

{{% notice note %}}
Creation and Deletion of Projects are privileged operations. This is typically performed by an Org Admin using the Web Console because RBAC assignments also need to be implemented along with this.
{{% /notice %}}

Prior to proceed with the setup, please navigate to [Rafay Home dashboard](https://console.rafay.dev/#/main) and identify the project number your have been assigned to (it is the number in *aws-workshop-xx* project name). Please use it instead of *xx* value for naming convention in the steps below.
![Rafay Project](/images/rafay_dashboard_project.png)


---

## Step 1: Create Cloud Credentials 

Cloud credentials provide privileges to programmatically interact with your Amazon AWS account so that the lifecycle of infrastructure associated with the Amazon EKS cluster can be managed by Rafay's Kubernetes Operations Platform. 

- Login to the Rafay Console with the credentials provided and click on *Infrastructure*

- Select *Cloud Credentials*, Click on ***New Credential*** and provide a unique name `aws-cloudcredential-xx` where xx is the Rafay project number assigned to you in AWS Organization
![Create Cloud Credential](/images/rafay_addcredential.png)

- Click on the dropdown for *Credential Type* and select **Role**

- Click the **Copy** button for the *External ID*. You will need it in the next step as the input for the CloudFormation template

- Click on the following [CloudFormation template link](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=Rafay-Cloud-Credential&templateURL=https://rafay-aws-workshop.s3.us-west-2.amazonaws.com/Rafay-Cloud-Credential.template). It opens AWS Management Console in the new browser tab under the credentials you have logged in before from Event Engine tool. You should see *Create Stack* page. This template creates cross-account access role for Rafay to perform necessary operations with IAM, CloudFormation, EKS, EC2 and Auto-scaling groups in your AWS account. Click **Next** to continue

- On *Specify Stack Details* page navigate to the *External ID* field and insert External ID value copied from Rafay *Add Credential* screen, click **Next**

- Scroll down of *Configure Stack Options* page and click **Next**

- On *Review Rafay-Cloud-Credential* page, scroll to the bottom and check the box that you acknowledge that the CloudFormation template might create an IAM resource and then click **Create Stack** 

- Wait for a few seconds and refresh the *Events* tab. Navigate to *Outputs* tab at the top of the page and copy the **RoleARN** value
![Copy RoleARN value](/images/cfnstack_outputs_rolearn.png)

- Return to the Rafay Console and paste the **RoleARN** value into *Role ARN* section in *Add Credential* window and hit **Save**
![Create Cloud Credential](/images/rafay_addcredential_rolearn.png)

---

## Step 2: Download and configure Rafay command line interface utility (RCTL)

The [RCTL CLI](https://docs.rafay.co/cli/overview/) allows you to programmatically interact with the controller enabling users to construct sophisticated automation workflows. 

Run the following commands in the Cloud9 instance to download and extract RCTL:

```
curl -s -o rctl-linux-amd64.tar.bz2 https://s3-us-west-2.amazonaws.com/rafay-prod-cli/publish/rctl-linux-amd64.tar.bz2
tar -xf rctl-linux-amd64.tar.bz2
chmod 0755 rctl
```

### Set Path for RCTL ###

After downloading the RCTL CLI, run the command below to add it to your OS's PATH environment variable. 

```
export PATH=$PATH:/home/ec2-user/environment
```

### Initialize RCTL ###
  
The RCTL utility needs to be initialized with credentials and other information before it can interact with the Controller.

RCTL supports both a "config file" as well as "dynamic config" model. The latter is well suited for automation pipelines where the configuration is provided dynamically and there is no need to permanently bind RCTL to an Org or Project. For today's workshop, we will use a "config file".

### Initialize RCTL Config File ###

- Navigate to the *[My Tools](https://console.rafay.dev/#/main/tools)* page in the Rafay Web Console

- Click on **Download CLI Config** to download the configuration file

- Save the configuration file on your local system

![CLI Tools Page](/images/cli_tools_page.png)

- In the Cloud9 interface, go to **File** -> **Upload Local Files...**

- Drag and drop or select the CLI config file that was previously downloaded

- In the Cloud9 terminal, run the following command to initialize RCTL

```
rctl config init "<CONFIG FILE NAME>"
```

{{% notice note %}}
At a given time, RCTL can be initialized with only one configuration. To learn more how to configure Rafay CLI please check the [documentation](https://docs.rafay.co/cli/config/).
{{% /notice %}}

{{% notice note %}}
If you receive *The requested URL /auth/v1/projects// was not found on this server* error please make sure you executed the previous step `export PATH=$PATH:/home/ec2-user/environment` command
{{% /notice %}}

{{% notice note %}}
If you receive *Error: accepts 1 arg(s), received 2* error please make sure you added double quotes around the config file name since *AWS Workshop* organization  has the space in the name , for example `rctl config init "AWS Workshop-user@amazon.com.json"`
{{% /notice %}}

### View RCTL Config ###

You can view the current configuration for RCTL by using the `rctl config show` command.

```
Profile:                                                                    prod
REST Endpoint:                                                 console.rafay.dev
OPS Endpoint:                                                      ops.rafay.dev
API Key:                                                            <Masked>
API Secret:                                                         <Masked>
Project:                                                          defaultproject
```

### Set Project

By default, the RCTL config points to the *defaultproject* as you saw on the previous step while viewing the config. 

For the purpose of this workshop each person will have their own project that will be provided along with the credentials. This step will ensure that you set the project context before you can perform operations in this project. For example, to set the project context to your Rafay project provided in the workshop run the below command with the name of the project provided

```
rctl config set project <NAME OF ASSIGNED PROJECT>
```

Once the project context has been successfully set, ensure you verify this in your local config file by running `rctl config show` again.

{{% notice note %}}
The name of the project is case-sensitive
{{% /notice %}}

---

## Step 3: Clone Git Repo 

{{% notice note %}}
A GitHub account is required for this step.  If you don't have a GitHub account, you can sign up [here](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home).
{{% /notice %}}

[Declarative specs](https://docs.rafay.co/clusters/eks/declarative/cli/#declarative) for the Amazon EKS cluster and other resources are available in a [Git repository](https://github.com/RafaySystems/aws-workshops).  

- Clone the Git repository to Cloud9 environment using the command below:
```
git clone https://github.com/RafaySystems/aws-workshops.git
```

- Once complete, you should see a folder called **kop_workshop** which contains the specs needed for this guide.
![Cloud9 Environment folder structure](/images/cloud9_setup.png)

--- 

## Recap

At this point, you have everything setup and configured for the workshop.

---
