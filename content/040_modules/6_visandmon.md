---
title: Part 6 - Visibility and Monitoring 
description: Learn Rafay Kubernetes Operation Platform (KOP) with EKS Workshop. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability.
chapter: true
weight: 16
---

## What Will You Do

This is part 6 of a multi-part workshop. In this section, you will explore the integrated visibility and monitoring capabilities of the platform. Specifically, you will explore the dashboards that provide you access to both critical summary and trends. 

- You will start with a "bird's eye view" and contextually click in one level at a time, going deeper and deeper. 
- Critical metrics are automatically scraped and aggregated at the controller in a centralized time series database (TSDB)
- Interactive, real time access to this data is provided


**Estimated Time**

Estimated time burden for this part is 15 minutes. 

!!! Important
    This part requires the "monitoring addon" to be enabled in the cluster blueprint. 

---

## Org Dashboard

Click on Home -> Dashboard to view all clusters, projects, user activity, resource utilization and events in the Project. You should see something like the example below.

![Org Dashboard](/040_modules/img/part6/org_dashboard.png)

---

## Project Dashboard

Select a Project and Click on Dashboard to view all clusters, projects, user activity, application, workloads and events across the entire Project. You should see something like the example below.

![Project Dashboard](/040_modules/img/part6/project_dashboard.png)

---

## Cluster Dashboard 

In your project, click on Infrastructure -> Clusters. You should see something like the example below providing an overview of critical, operational metrics for your cluster. 

![Cluster Dashboard](/040_modules/img/part6/cluster_dashboard.png)

--- 

## Node Dashboard

In the cluster dashboard, click on Nodes. This will provide you an overview of all the nodes in the cluster. You should see something like the example below.

![Node Dashboard](/040_modules/img/part6/node_dashboard.png)

Click on "Overview" for one of your nodes. This will provide you with a dashboard for the node.

![Node Dashboard Details](/040_modules/img/part6/node_dashboard_trends.png)

---

## k8s Resources 
Click on the "Resources" tab. This will provide you access to an "integrated Kubernetes dashboard" where you can view the k8s resources organized by type, by namespace etc. 

![Kubernetes Dashboard](/040_modules/img/part6/k8s_resources.png)

--- 

## Pod Dashboard

By default, the k8s dashboard will list the pods in the "kube-system" namespace. Click on the "coredns" pod to view the Pod dashboard. You should see something like the example below. 

![Pod Dashboard](/040_modules/img/part6/pod_dashboard.png)

---

## Container Dashboard 

Click on "Configuration" for the pod in the pod dashboard

![Pod Configuration](/040_modules/img/part6/pod_config.png)

Now, click on the container in the configuration to go to the "container dashboard". You should see something like the example below.

![Container Dashboard](/040_modules/img/part6/container_dashboard.png)

---

## Recap

Congratulations! At this point, you have successfully accessed the integrated dashboards for visibility and monitoring.

---
