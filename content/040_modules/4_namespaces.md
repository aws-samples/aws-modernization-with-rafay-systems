---
title: Part 4 - Namespaces 
description: Learn Rafay Kubernetes Operation Platform (KOP) with EKS Workshop. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability.
chapter: true
weight: 14
---

## What Will You Do

This is part 4 of a multi-part workshop.  In this section you will: 

- Configure a Kubernetes namespace spec in your project 
- Publish this namespace to a Kubernetes cluster

**Estimated Time**

Estimated time burden for this part is 5 minutes. 

---

## Step 1: Create Namespace 

- Select Infrastructure -> Namespaces
- Click on "New Namespace"
- Enter "amazon-cloudwatch" for ***Name***
- Select "Wizard" for ***Type*** and Save 

![Create Namespace](/040_modules/img/part4/create_ns.png)


!!! Note:
In addition to the Namespace wizard, users can also provide the k8s YAML spec for the namespace either by uploading it or point the controller to a Git repo where it can retrieve it. 

---
## Step 2: Configure Namespace 

You will be presented with an intuitive wizard that you can use to configure your namespace's requirements. In our case, we want to add labels to our namespace. 

- Click on Labels -> Key-Value
- Provide k8s compliant text for the key and value 
- Save 

In the example below, we have entered "key=addon" and "value=cloudwatch" 

![Configure Namespace](/040_modules/img/part4/configure_ns.png)

--- 
## Step 3: Select Placement  

We have the ability to place the namespace on multiple clusters through the fleet management capabilities of the platform.  For this excercise, we will place the namespace on the imported cluster only for now.

- Select "Specific Clusters" for Placement Policy 
- Select your imported cluster 
- Click "SAVE & GO TO PUBLISH"

![Place Namespace](/040_modules/img/part4/place_ns.png)

---
## Step 4: Publish Namespace

Click on Publish. In a few seconds, the configured namespace will be deployed on the target cluster. Note that multiple target clusters can be in completely separate security domains and the controller can still manage namespace lifecyle remotely. 

![Publish Namespace](/040_modules/img/part4/publish_ns.png)

---

## Step 5: Verify Namespace

Optionally, you can verify what the published namespace looks like on your cluster. 

- Navigate to Infrastructure -> Clusters 
- Click on Kubectl 

In the example below, you can see that the "amazon-cloudwatch" namespace was created on the cluster a few seconds back when we published it. 

``` yaml hl_lines="8"
kubectl get ns 

NAME                STATUS   AGE
amazon-cloudwatch   Active   12s
default             Active   26h
kube-node-lease     Active   26h
kube-public         Active   26h
kube-system         Active   26h
rafay-infra         Active   14m
rafay-system        Active   15m
```

You can also look deeper into the namespace by describing it. Notice that the the "custom label" we specified is part of the namespace.

``` yaml hl_lines="4"
kubectl describe ns amazon-cloudwatch

Name:         amazon-cloudwatch
Labels:       addon=cloudwatch
              app.kubernetes.io/managed-by=Helm
              kubernetes.io/metadata.name=amazon-cloudwatch
              name=amazon-cloudwatch
              rafay.dev/auxiliary=true
              rafay.dev/component=namespace
              rafay.dev/global=true
              rafay.dev/name=namespace
              rep-cluster=mx6vp0m
              rep-cluster-name=basic-eks
              rep-drift-reconcillation=disabled
              rep-organization=1ky5702
              rep-partner=rx28oml
              rep-placement=28104lk
              rep-project=3mx397m
              rep-project-name=aws-workshop-01
              rep-system-managed=true
              rep-workload=namespace-k6wzqwm-amazon-cloudwatch
              rep-workloadid=kg1637k
Annotations:  meta.helm.sh/release-name: namespace-k6wzqwm-amazon-cloudwatch
              meta.helm.sh/release-namespace: amazon-cloudwatch
              rafay.dev/resource-hash: 9fb4dd6c47c9edbd27b73b28bce1913e6ba021ee2d0fdfe54c956822390bd45e
              rep-drift-action: deny
Status:       Active

No resource quota.

No LimitRange resource.
```

---

## Recap

Congratulations! At this point, you have successfully configured and published a namespace to your Amazon EKS clusters. You also verified the namespace's specification directly on the cluster using Kubectl. 

---
