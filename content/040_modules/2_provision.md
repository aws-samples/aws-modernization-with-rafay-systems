---
title: Part 2 - Provision and Import EKS Clusters 
description: Learn Rafay Kubernetes Operation Platform (KOP) with EKS Workshop. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability.
chapter: true
weight: 12
---


## What Will You Do

This is part 2 of a multi-part workshop.  In this section of the workshop you will provision a new Amazon EKS cluster and import an existing Amazon EKS cluster.

---

## Step 1: Configure and Provision Amazon EKS Cluster

In this step, you will configure and customize your Amazon EKS Cluster specification using a YAML based cluster specification.

Provisioning will take approximately 40 minutes to complete. The final step in the process is the blueprint sync for the default blueprint. This can take a few minutes to complete because this requires the download of several container images and deployment of monitoring and log aggregation components.

- In the left hand pane of the Cloud9 instance, right click and select "New File".  Right clikc the new file and rename it to "eks-cluster-basic.yaml"
- Open the file and add the cluster spec details below

- Save the below specification file to your computer as "eks-cluster-basic.yaml"

``` yaml
kind: Cluster
metadata:
  name: cloudwatch-cluster
  project: aws-workshop-xx
  labels:
    env: dev
    type: eks-workloads
spec:
  blueprint: default
  cloudprovider: dev-aws
  cniprovider: aws-cni
  type: eks
---
apiVersion: rafay.io/v1alpha5
kind: ClusterConfig
metadata:
  name: cloudwatch-cluster
  region: us-east-1
  tags:
    'demo': 'true'
  version: "1.20"
managedNodeGroups:
  - name: ng-1
    instanceType: t3.large
    desiredCapacity: 2
    iam:
     withAddonPolicies:
      albIngress: true
      autoScaler: true
      efs: true
      cloudWatch: true
```

Update the following sections of the specification file with details to match your environment

- Update the project section with the name of the project in your organization
``` yaml
    project: aws-workshop-xx
```

- Update the cloudprovider section with the name of the cloud credential that was previously created
``` yaml
    cloudprovider: dev-aws
```
- Save the file

- Execute the following command to provision the cluster from the specification file previously defined
```
rctl apply -f eks-cluster-basic.yaml
```
***Expected output (with a task id):***

```
{
  "taskset_id": "3mx63om",
  "operations": [
    {
      "operation": "NodegroupCreation",
      "resource_name": "ng-1",
      "status": "PROVISION_TASK_STATUS_PENDING"
    },
    {
      "operation": "ClusterCreation",
      "resource_name": "cloudwatch-cluster",
      "status": "PROVISION_TASK_STATUS_PENDING"
    }
  ],
  "comments": "The status of the operations can be fetched using taskset_id",
  "status": "PROVISION_TASKSET_STATUS_PENDING"
}
```

To retrieve the status of the apply operation, enter the below command with the generated task id
```
rctl status apply d2wg4k8
```

***Expected Output***

```
{
  "taskset_id": "3mx63om",
  "operations": [
    {
      "operation": "NodegroupCreation",
      "resource_name": "ng-1",
      "status": "PROVISION_TASK_STATUS_PENDING"
    },
    {
      "operation": "ClusterCreation",
      "resource_name": "cloudwatch-cluster",
      "status": "PROVISION_TASK_STATUS_INPROGRESS"
    }
  ],
  "comments": "Configuration is being applied to the cluster",
  "status": "PROVISION_TASKSET_STATUS_INPROGRESS"
}
```

- Login to the web console and view the cluster being provisioned

![Create Cluster](img/part2/cluster-provision-1.png)

Once the cluster finishes provisioning, download the cluster configuration file and compare it to the specification file used to create the cluster.  The two files will match.

- Go to Clusters -> Infrastructure.  
- Click on the Settings Icon for the newly created cluster and select "Download Cluster Config"

---

### Step 2 : Import an existing Amazon EKS Cluster

In this step, you will import an existing Amazon EKS cluster

- Run the following commands to import the cluster

```
rctl create cluster imported basic-eks > basic-eks-bootstrap.yaml
kubectl apply -f basic-eks-bootstrap.yaml
```

You can monitor progress/status by clicking the cluster in the Rafay console.

Once this step is complete, you should be able to view the cluster details in the console. 

![Imported EKS Cluster](img/part2/eksa_cluster_rafay.png)

---

## Recap

Congratulations! At this point, you have

- Successfully configured and provisioned an Amazon EKS cluster and imported an existing Amazon EKS Cluster. 

---
