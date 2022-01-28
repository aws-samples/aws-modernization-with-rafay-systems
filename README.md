# AWS Modernization with Rafay Systems

This is a Rafay / AWS guided workshop in which participates will learn how to automate the operations of Amazon EKS clusters through Rafay's Kubernetes Operations Platform. The workshop's targeted audience is cloud operations teams, DevOps engineers, Site Reliability Engineers (SREs), or other infrastructure leads that are managing the lifecycle operations of Kubernetes for their organization.

## Building the Website

This page is built with Hugo, so you'll need it [installed](https://gohugo.io/getting-started/quick-start/#step-1-install-hugo)

First, clone this repo:

```bash
git clone git@github.com:aws-samples/aws-modernization-with-rafay-systems.git
```

Ensure you've also cloned the submodules:

```bash
git submodule init
git submodule update
```

Then server the website with hugo:

```bash
hugo server

```

### Learning Objectives
- How to provision a new EKS cluster with Rafay Kubernetes Operations Platform and import existing clusters for centralized operations.
- How to create centralized configurations for the look and feel of EKS clusters being managed across an enterprise.
- How to automate the upgrade of a new Kubernetes version across EKS clusters.
- How to enable centralized zero trust access into Kubernetes clusters.
- How to audit access into EKS clusters and changes being made to EKS cluster
