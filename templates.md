---
title: Templates
filename: templates.md
--- 
# Template Docs

Compoze Service Catalog allows you to provide your own custom Golden Templates. This allows your organization to provide your own application code standards and defaults, while still leveraging the power of the Compoze platform. 

Compoze supports most modern languages, as long as it can be properly dockerized or bundled. There are four types of applications that Compoze supports

1. API service
2. Web App service
3. Fullstack (API & Web App) service
4. Fullstack Serverless (Amplify Web App & Lambda APIs)

## API Service Template Guidelines

In order to create a Golden Template for an API service, that will work with the Compoze Service Catalog follow the below guidelines:

### Application Requirements

In order to expose a container externally, via an Application Load Balancer, a port must be exposed for the load balancer to route traffic to. Compoze assumes port 5000, so your API must be running on that port.

### Replacement Values

The Compoze Service Catalog template engine works by replacing template value names (such as the service name) before adding the template to the Github Repository created for you. The key value pairs are the currently supported replacement values

| Template Key             | Replacement Value                                                      |
| ------------------------ | ---------------------------------------------------------------------- |
| {% raw %}{{COMPOZE_SERVICE_NAME}}{% endraw %} | The name of the service you configured via the Compoze Service Catalog |
| {% raw %}{{AWS_REGION}}{% endraw %}           | AWS Region name that your service has been created in                  |
| {% raw %}{{AWS_ACCOUNT_ID}}{% endraw %}       | AWS Account ID that your service is created in                         |
| {% raw %}{{COMPOZE_PRODUCT_NAME}}{% endraw %} | The name of the Compoze Product that the service is created in         |

### Environment Files

Compoze will also inject environment variable files (i.e. prod.env) that contain values required to build, deploy, and test your service. The files will be placedin an **environments** folder at the root of your project. The following key value pairs will be provided in each environment file:

```env
ECR_REPOSITORY_URL=...
ECS_SERVICE_NAME=...
ECS_SERVICE_ARN=...
ECS_CLUSTER_NAME=...
ECS_CLUSTER_ARN=...
ECS_TASK_DEFINITION_NAME=...
ECS_TASK_DEFINITION_ARN=...
BASE_DNS_URL=...
ENVIRONMENT=...
```

You should use these values to build any automation scripts required to build, test, and deploy your appliction

### Application Requirements

The only current requirement is that your application, when bundled inside a docker image, is exposed on port **5000**. This is needed to forward traffic from the Compoze configured Load Balancer to your container running in ECS

### Github Actions

You can also choose to provde your own Github Actions file to orchestrate your continuous delivery pipeline. There are a few recommendations and restrictions:

1. **Environment Variables:** Environment specific variables are provided in seperate .env files (i.e. prod.env, dev.env, etc). In order to access these variables, for things like deploying to a specific environment, you can either include a ```source X.env``` command in your scripts, or configure your Github Actions configuration file to source the variables before. Below is an example of how you might configure that:

```yaml
name: Deploy to production

on:
  push:
    branches:
      - main

env:
{% raw %}
  ECR_REPOSITORY_URL: ${{ env.ECR_REPOSITORY_URL }}
  ECS_SERVICE_ARN: ${{ env.ECS_SERVICE_ARN }}
{% endraw %}

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
      
    - name: Set environment variables
      run: echo "::set-env name=ECR_REPOSITORY_URL::$(cat prod.env | grep ECR_REPOSITORY_URL | awk -F '=' '{print $2}')"
      run: echo "::set-env name=ECS_SERVICE_ARN::$(cat prod.env | grep ECS_SERVICE_ARN | awk -F '=' '{print $2}')"
      
    - name: Deploy to production
      run: |
      {% raw %}
        echo "ECR Repository URL: ${{ env.ECR_REPOSITORY_URL }}"
        echo "ECS Service ARN: ${{ env.ECS_SERVICE_ARN }}"
        {% endraw %}

        # ... Deployment commands here ...
```

2. **AWS Access:** In order to deploy your application to the Compoze managed AWS Infrastructure, in your account, you need to provide access to the deployment scripts in your Github Actions pipelines. Compoze ***strongly*** encourages you to not use AWS Access Key credentials to do this, but rather to rely on the pre-configured Github OIDC deployments. Since Compoze has already configured this connection for you, all you need to do is use the Github [configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials) plugin. Below is an example deployment step that uses the plugin:

```yaml
deployprod:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v3
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          role-to-assume: arn:aws:iam::<AWS_ACCOUNT_ID>:role/CompozeAutomationRole
          aws-region: <AWS_REGION>
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - uses: pnpm/action-setup@v2
        with:
          version: 7.18.2
      - name: Deploy prod
        run: pnpm run deploy prod
        env:
          ENV: <ENVIRONMENT>
          REGION: <AWS_REGION>
          AWS_ACCOUNT_ID: <AWS_ACCOUNT_ID>
```

Compoze preconfigures your CompozeAutomationRole to allow Github to assume this role via [OIDC](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services), so no other configuration is required.

3. **Deployment Stages:** During the Compoze Product creation process, you will configure the number of environments that will be created for your Services; for example dev, stage, and production environments. In order to deploy to each of these environments, your Github Actions files must contain deployment stages for each environment. 
4. **Github Actions Configuration:** The standard placement for your Github Actions file to be picked up by Github is in a **.github** directory in the root of your project. Generally, this is labeled something like main.yaml, though the exact naming convention can be left to your discretion


## Fullstack (API & Web App) service

In order to create a Golden Template for a Fullstack application, that will work with the Compoze Service Catalog, follow the below guidelines:


### Application Requirements

In order to expose a container externally, via an Application Load Balancer, a port must be exposed for the load balancer to route traffic to. Compoze assumes port 5000, so your API must be running on that port.

### Application Structure 

Compoze Service Catalog template engine assumes the following folder structure for you Fullstack repository:

api/
ui/

Environment files will be places in their respective directories, as described in the **Environment Files** section below

### Replacement Values

The Compoze Service Catalog template engine works by replacing template value names (such as the service name) before adding the template to the Github Repository created for you. The key value pairs are the currently supported replacement values

| Template Key             | Replacement Value                                                      |
| ------------------------ | ---------------------------------------------------------------------- |
| {% raw %}{{COMPOZE_SERVICE_NAME}}{% endraw %} | The name of the service you configured via the Compoze Service Catalog |
| {% raw %}{{AWS_REGION}}{% endraw %}           | AWS Region name that your service has been created in                  |
| {% raw %}{{AWS_ACCOUNT_ID}}{% endraw %}       | AWS Account ID that your service is created in                         |
| {% raw %}{{COMPOZE_PRODUCT_NAME}}{% endraw %} | The name of the Compoze Product that the service is created in         |


### Environment Files

Compoze will also inject environment variable files (i.e. prod.env) that contain values required to build, deploy, and test your service. The files will be placedin an **environments** folder at the root of each respective project (api & ui). The following key value pairs will be provided in each environment file:

#### api

```env
ECR_REPOSITORY_URL=...
ECS_SERVICE_NAME=...
ECS_SERVICE_ARN=...
ECS_CLUSTER_NAME=...
ECS_CLUSTER_ARN=...
ECS_TASK_DEFINITION_NAME=...
ECS_TASK_DEFINITION_ARN=...
BASE_DNS_URL=...
ENVIRONMENT=...
```

#### ui

```env
AMPLIFY_APP_NAME=...
AMPLIFY_APP_ID=...
BASE_DNS_URL=...
ENVIRONMENT=...
```

## Web App Services Template Guidlines

Coming Soon

