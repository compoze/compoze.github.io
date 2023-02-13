---
title: Compoze
filename: README.md
--- 
# Compoze Platform

Compoze is a SaaS-based infrastructure management and Platform Ops tool that enables your teams to build Performant, Secure, Cost Effective, Operationally Excellent, and Reliable applications on AWS, in minutes. Compoze is built with two core tenants:

1. Golden AWS Account
2. Service Catalog

## Golden AWS Account

In order to even begin to deploy applications within AWS, there are core foundational services that must be setup in order to ensure your applications are production-grade. Creating these core AWS services involves a comprehensive approach to ensure operational excellence, cost optimization, security, performance, and reliability are maintained. The work required to achieve this includes:

1. **Operational Excellence:** This involves setting up processes and tools to manage the day-to-day operations of the services. This includes tasks such as setting up automation, monitoring, logging, and alerting.

2. **Cost Optimization:** This involves controlling and reducing the cost of running the services. This can include cost allocation tagging, reserved instances, and cost analysis.

3. **Security:** This involves implementing security measures to protect data and systems. This can include access controls, network security, encryption, and identity and access management.

4. **Performance:** This involves optimizing the performance of the services. This can include using the right instance types, auto scaling, and proper load balancing.

5. **Reliability:** This involves ensuring the services are available and reliable. This can include setting up disaster recovery, auto healing, and multi-availability zone deployments.

Normally, each of these areas requires a combination of planning, development, testing, and deployment activities. The teams involved may include developers, operations, security, and infrastructure teams. Ongoing maintenance and updates are also required to ensure that the services continue to meet the goals of operational excellence, cost optimization, security, performance, and reliability.

Compoze will configure, maintain, and operate the AWS Services necessary to give you the proper foundation to build your applications.

## Service Catalog

Compoze' Service Catalog is a platform that provides tools for your engineering teams to build and manage their AWS applications, inluding APIs, Websites, Full Stack services, and more. It provides a centralized location for teams to discover, publish, and manage their applications, as well as tools to monitor performance and security. This enables teams to work more efficiently and focus on delivering high-quality services.

Compoze will help your teams build and manage your applications by:

**Deploying resources:** You can deploy resources, such as servers, databases, and networking, directly from the Compoze interface. Simply select the type of applications you want to deploy and Compoze will do the rest.

**Managing resources:** Once you have deployed your resources, you can manage them from the Compoze interface. This includes updating configurations, scaling resources, and more.

**Automating tasks:** Compoze allows you to automate routine tasks, such as backups, updates, and scaling. This can save you time and ensure that your infrastructure is always running smoothly.

### Compoze Service Catalog Services

A Compoze Service is made up of the following areas

1. A Github repository
2. A golden source code template
3. CI/CD pipeline
4. AWS Infrastructure to host, manage, and support your application

When a new service is created, Compoze will perform the following steps:

1. Create a new repository in your specified Github Organization
2. Inject your new repository with a golden template. These templates can either be a:
   1. Compoze managed template
   2. Your own custom template
3. Update your template with supported values. These values include:
    1. App name
    2. Region
    3. AWS Account Id
    4. Compoze Product Name
4. Inject environment variable files (for each of your configured environments) with values like:
    1. Base DNS Name
    2. Environment Name
    3. Infrastructure resources such as ECR repository, ECS Cluster details, etc
5. Deploy the necessary AWS Infrastructure for you service
6. Configure Github Actions pipeline
7. Execute your Github Actions pipeline

## Getting Started
To get started with Compoze, you will first need to create an account. To do this, follow these steps:

1. Create an [AWS Account](https://aws.amazon.com/free)
2. Register a Route53 Domain
3. Create an IAM Role called CompozeAutomation
4. Apply Compoze IAM Policies to IAM Role
5. Enable [billing access](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html) for IAM Users
   
## Support
If you have any questions or need help with Compoze, you can reach out to our support team at [help@compoze.io](help@compoze.io)