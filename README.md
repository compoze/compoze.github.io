# Compoze Platform

Compoze is a SaaS-based infrastructure management and Platform Ops tool that enables your teams to build production applications in minutes. Compoze makes it easy to build Performant, Secure, Cost Effective, Operationally Excellent, and Reliable applications on AWS. Compoze is built with two core tenants:

1. AWS Golden Template
2. Service Catalog

## Golden Template

In order to even begin to deploy applications within AWS, there are core foundational services that must be setup in order to ensure your applications are production-grade. Creating these core AWS services involves a comprehensive approach to ensure operational excellence, cost optimization, security, performance, and reliability are maintained. The work required to achieve this includes:

1. **Operational Excellence:** This involves setting up processes and tools to manage the day-to-day operations of the services. This includes tasks such as setting up automation, monitoring, logging, and alerting.

2. **Cost Optimization:** This involves controlling and reducing the cost of running the services. This can include cost allocation tagging, reserved instances, and cost analysis.

3. **Security:** This involves implementing security measures to protect data and systems. This can include access controls, network security, encryption, and identity and access management.

4. **Performance:** This involves optimizing the performance of the services. This can include using the right instance types, auto scaling, and proper load balancing.

5. **Reliability:** This involves ensuring the services are available and reliable. This can include setting up disaster recovery, auto healing, and multi-availability zone deployments.

Normally, each of these areas requires a combination of planning, development, testing, and deployment activities. The teams involved may include developers, operations, security, and infrastructure teams. Ongoing maintenance and updates are also required to ensure that the services continue to meet the goals of operational excellence, cost optimization, security, performance, and reliability.

Compoze will configure, maintain, and operate the AWS Services necessary to give you the proper foundation to build your applications

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
4. Required AWS Infrastructure to host, manage, and support your application

When a new service is created, Compoze will perform the following steps:

1. Create a new repository in your specified Github Organization
2. Inject your new repository with a golden template. These templates can either be:
   1. Compoze managed tempalte
   2. Your own custom template
3. Update your template with supported values
    1. App name
    2. Region
    3. AWS Account Id
    4. Compoze Product Name
4. Inject environment variable files (for each of your configured environments) with:
    1. ECR Repository URL
    2. ECS Service ARN
    3. ECS Service Name
    4. ECS Cluster ARN
    5. ECS Cluster Name
    6. ECS Task Defintion Arn
    7. ECS Task Defintion Name
    8. Base DNS Name
    9. Environment Name
5. Deploy the necessary AWS Infrastructure for you service
6. Configure Github Actions pipeline. Compoze requires the following commands be exposed from your package.json. Note that currently, Compoze supported version of Node requires pnpm
    1. pnpm install
    2. pnpm build 
        1. This command should build the application bundle
    3. pnpm test   
        1. This command should execute unit tests, and any other self contained tests
    4. pnpm deploy  
        1. This command should build the application bundle, build a docker container, publish to the ECR container, update the ecs service to force a new task definition, wait for secs service to become stable
7. Execute your Github Actions pipeline

## Getting Started
To get started with Compoze, you will first need to create an account. To do this, follow these steps:

1. Go to the Compoze website at https://compoze.io
2. Click on "Sign Up" in the top right corner
3. Fill out the registration form with your information
4. The Compoze team will reach out with your account credentials
5. Log in to your account
   
## Support
If you have any questions or need help with Compoze, you can reach out to our support team at [help@compoze.io](help@compoze.io)