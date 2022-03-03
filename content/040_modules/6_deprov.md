---
title: Part 6 - Deprovision Cluster
description: Official Rafay product documentation. Explore "Learn KOP - Deprovision Cluster" docs and more here. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability.
chapter: true
weight: 20
---


## What Will You Do

This is the part 6 of a multi-part workshop.  In this section, you will deprovision your EKS cluster and all infrastructure that was deployed to your AWS account. 

---

## Deprovision

You can deprovision/delete your EKS cluster using the RCTL CLI. 

- Run the following commands to delete the clusters.  Replace the "xx" with the number of your project.

```
rctl delete cluster imported-cluster-xx
rctl delete cluster provisioned-cluster-xx
```

Alternatively, you can also perform this from the web console

- Click on the gear icon on the far right of the cluster 
- Select "Delete" and acknowledge 

NOTE that the delete operation can take some time so that it can delete all the infrastructure associated with your EKS Cluster. 


--- 
