---
title: Part 7 - Deploying a Workload 
description: Learn Rafay Kubernetes Operation Platform (KOP) with EKS Workshop. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability. 
chapter: true
weight: 17
---

## What Will You Do

This is part 7 of a multi-part workshop.  In this part, you will deploy a workload to an Amazon EKS cluster. This part assumes the user has an account on [Github](https://www.github.com) and will fork the repository as a Public repository. 

**Estimated Time**

Estimated time burden for this part is 10 minutes. 

---

## Step 1: Fork Repository 

To setup a GitOps pipeline, you will need a Git repository. To help you get started, let us fork an existing repository. 

- Ensure you are logged into your GitHub.com account 
- Navigate to the public [Git repository](https://github.com/RafaySystems/demo-apps)
- Click on Fork repository 
- Select your account name to fork repo to 

This will create a copy of the repository in your Git system (e.g. GitHub). An example is shown below.

![Forked Git Repository](/040_modules/img/part7/forked_repo.png)

---

## Step 2: Add Repository

- In the desktop project, navigate to Integrations -> Repositories
- Click on New Repositories
- Provide a friendly name
- Select Git for Type

![New Repository](/040_modules/img/part7/new_repo.png)

- Provide the Git repo's Endpoint URL 
- Save 

![Git Endpoint](/040_modules/img/part7/configure_repo.png)

!!! Note
    It does not matter if your Github repo is public or private. If private, you need to provide access credentials. 

Optionally, 

- Under Infrastructure -> Repositories, Click on validate for your repository 
- If you see a validation successful message, the controller is able to access the repository 

![Validate Repository](/040_modules/img/part7/validate_repo.png)

---

## Step 3: Create Namespace

We need to create a namespace to deploy our workload via the GitOps pipeline  

- Navigate to Infrastructure -> Namespaces
- Click on New Namespace
- Provide a name, select wizard for type and Save
- Under placement, select your cluster and publish the namespace 

In the example below, we have created a namespace called "first" on our cluster. 

![Create Namespace](/040_modules/img/part7/publish_namespace.png)

--- 

## Step 4: Create Workload

Now, we are ready to create a workload based on k8s manifests in our Git repository and publish it to the namespace we just created. 

- Navigate to Applications -> Workloads
- Click on New Workload and provide a friendly name
- Select "Helm 3" for package type
- Select "Pull files from repository" for Artifact Sync
- Select "Git" for Repository Type
- Select the namespace you created in the previous step and Select "Continue"

![Create Workload](/040_modules/img/part7/new_helm3_workload.png)

---

## Step 5: Publish Workload

We are now ready to configure our workload by (a) specifying the Git repo and (b) selecting the cluster for placement. 

- Select the name of the repository created in the previous steps
- Select "master for revision 
- Enter path for Helm file i.e. "Helm/webserver" 
- Enter path for the values file i.e. "Helm/webserver/values.yaml" and Select "Continue"

![Configure Workload](/040_modules/img/part7/config_helm3_workload.png)


- Select your cluster for Placement Policy and Publish workload. 

In a few seconds, the workload's k8s resource will become operational on your cluster. 

![Published Workload](/040_modules/img/part7/published_helm3_workload.png)

Optionally, you can also verify the status of the resources using the zero trust kubectl channel. You should see something like the following 

``` yaml
kubectl get po -n first

NAME                                   READY   STATUS    RESTARTS   AGE
test-helm-webserver-7874b96c7c-tzpqt   2/2     Running   0          10s
```

--- 

## Recap

Congratulations! At this point, you have successfully created a workload and deployed it to an EKS cluster.  

---
