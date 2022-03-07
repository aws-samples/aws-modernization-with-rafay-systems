---
title: Rafay Kubernetes Operations Platform Overview Workshop 
description: Learn the basics of how Rafay's Kubernetes Operation Platform (KOP) helps simplify the operations of modern applications running on Amazon EKS. Rafay is a SaaS-first Kubernetes Operations Platform with enterprise-class scalability.
chapter: true
weight: 5
---

In this workshop, you will learn how to standardize the configuration, deployment, and lifecycle management of Amazon EKS clusters through Rafay's Kubernetes Operations Platform.

---

### Prerequisites

{{% notice note %}}
Assumption is you are participating in a Rafay / AWS guided Workshop and have been provided credentials and access to preconfigured Rafay and AWS accounts as well as went through [Event Engine Setup](020_event_engine_setup.html) module
{{% /notice %}}

{{% notice tip %}}
During this workshop you will work with the several tools in parallel. Please open those tools in separate browser tabs and keep them open during the workshop. It is also helpful to keep this workshop guidance open on the one screen and perform hands-on lab on the second screen if you have it.
{{% /notice %}}

- [Rafay Dashboard](https://console.rafay.dev/) - login to Rafay using the email invitation you have received prior this event and make sure you are in **AWS Workshop** organization in the top right corner under your account email
- [Event Engine](020_event_engine_setup/20_aws_event_engine.html) which provides you access to the AWS account with pre-provisioned resources. Keep it open in a case you need to open AWS Management Console again or need AWS credentials
- [Cloud9 IDE](020_event_engine_setup/22_start_cloud9workspace.html) access where you will execute commands to configure AWS resources and run Rafay tools
- GitHub account to clone prepared EKS configuration. If you don’t have a GitHub account, you can sign up [here](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home).

---

During next steps we will learn how to setup preconfigured Rafay account with provided AWS account and how to solve some of Kubernetes management challenges with Rafay

{{% children showhidden="false" %}}

---