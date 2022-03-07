---
title: Part 5 - Amazon CloudWatch Blueprint 
description: Learn Rafay Kubernetes Operation Platform (KOP) with EKS Workshop. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability. 
chapter: true
weight: 15
---

In this section of the workshop you will create a custom cluster blueprint with a [Amazon CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) add-on, based on declarative specifications. 

<!--
TODO: Rafay team - why we are teaching this, what problem we are trying to solve
-->

---

## Step 1: Create Repository  

In this step, you will create a repository in your project so that the controller can retrieve the Helm charts automatically. 

Go to Cloud9 and navigate to the folder where you forked the Git repository */aws-workshops/kop-workshop/repository* and open *cloudwatch-repository.yaml* file. This file contains the declarative specification for the repository. In this case, the specification is of type **Helm Repository** and the *endpoint* is pointing to the AWS Github repository that includes the CloudWatch Helm chart. 
![CloudWatch Repository configuration file](/images/cloudwatch-repository.png)

- Type the command below to create the repository

```
rctl create repository -f cloudwatch-repository.yaml
```

If you did not encounter any errors, you can optionally verify if everything was created correctly on the controller.

- Navigate to your aws-workshop project

- Select **Integrations -> Repositories**

![Repository](/images/add_repository.png)

---

## Step 2: Create Addon

In this step, you will create a custom addon for the Cloudwatch Agent. Navigate to the folder */aws-workshops/kop-workshop/addon* and find 2 files:

- *cloudwatch-addon.yaml* file contains the declarative specification for the addon

- *custom-values.yaml* file is used as an override

Let's open both and inspect. The following details are used to build the declarative specification:

- *v1* because this is our first version

- Name of addon is *cloudwatch-addon*

- The addon will be deployed to a namespace called *amazon-cloudwatch*

- The *aws-cloudwatch-metrics* chart will be used from the previously created repository named *cloudwatch-repo*

![CloudWatch Agent Addon](/images/rafay-cloudwatch-addon.png)

Let's update files with our configuration:

- Update the following section of the specification file with details to match your environment. Replace the *xx* with the number of your project.
```
- project: aws-workshop-xx
```
- Update the *custom-values.yaml* file with the name of your imported cluster
```
- clusterName: imported-cluster-xx
```

- Type the command below to create the addon

```
rctl create addon version -f cloudwatch-addon.yaml
```

If you did not encounter any errors, you can optionally verify if everything was created correctly on the controller.

- Navigate to Rafay dashboard and your project

- Select **Infrastructure -> Addons** 

- You should see an addon called **cloudwatch-addon**

![CloudWatch Agent Addon](/images/cloudwatch_addon.png)

---

## Step 3: Create Blueprint

In this step, you will create a custom cluster blueprint with the CloudWatch addon. Navigate to */aws-workshops/kop-workshop/blueprint* folder. The *cloudwatch-blueprint.yaml* file contains the declarative specification for the blueprint.
![CloudWatch Agent Addon](/images/rafay-cloudwatch-blueprint.png)

- Update the following section with details to match your environment. Replace the *xx* with the number of your project.
```
- project: aws-workshop-xx
```

- Type the command below to create the blueprint

```
rctl create blueprint -f cloudwatch-blueprint.yaml
```

If you did not encounter any errors, you can optionally verify if everything was created correctly on the controller.

- Go to your "aws-workshop-xx" project and select **Infrastructure -> Blueprint**

- You should see the blueprint called "cloudwatch-blueprint

![CloudWatch Blueprint](/images/cloudwatch_blueprint.png)

---

###  Blueprint Version

Although we have a custom blueprint, we have not provided any details on what it comprises. In this step, you will create and add a new version to the custom blueprint. The YAML below is a declarative spec for the new version.  

- Open *cloudwatch-blueprint-v1.yaml* file and update the following section with details to match your environment. Replace the *xx* with the number of your project.
```
- project: aws-workshop-xx
```

- Type the command below to create a new blueprint version

```
rctl create blueprint version -f cloudwatch-blueprint-v1.yaml
```

If you did not encounter any errors, you can optionally verify if everything was created correctly on the controller.

- Select **Infrastructure -> Blueprint** in your project

- Click on the **cloudwatch-blueprint** custom cluster blueprint
![v1 CloudWatch Blueprint](/images/cloudwatch_blueprint_newversion.png)

---

## Step 4: Apply Cluster Blueprint

We will use Rafay CLI (rctl) to apply the newly created blueprint to the imported cluster.

- Edit the file *imported-cluster-bootstrap.yaml* that was previously created when importing the cluster

- Update the cluster blueprint section of the file with the name of the newly created blueprint *cloudwatch-blueprint*

- Under the blueprint section add the blueprint version to spec file
``` yaml
    blueprint: cloudwatch-blueprint
    blueprintversion: v1
```

- Save the file and execute the following command to update the cluster with the newly created blueprint
```
rctl apply -f imported-cluster-bootstrap.yaml
```

***Expected output (with a task id):***

```
{
  "taskset_id": "dk69p0k",
  "operations": [
    {
      "operation": "BlueprintUpdation",
      "resource_name": "imported-cluster",
      "status": "PROVISION_TASK_STATUS_PENDING"
    }
  ],
  "comments": "The status of the operations can be fetched using taskset_id",
  "status": "PROVISION_TASKSET_STATUS_PENDING"
}
```

## Recap

As of this step, you have created and applied a cluster blueprint with the CloudWatch agent as one of the addons.

Note that you can also reuse this cluster blueprint for as many clusters as you require in this project and also share the blueprint with other projects. 

---
