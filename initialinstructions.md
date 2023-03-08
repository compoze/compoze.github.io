---
title: InitialInstructions
filename: initialinstructions.md
---
# Initial Instructions

Before you are off and running with Compoze, there are a few one-time set up items to take care of. This allows the Compoze platform all the access rqeuired to full leverage all that Compoze has to offer.

## Setup Steps

### Step 1. Register a Route 53 Domain

You will need to register a domain through AWS Route 53. This will be used by Compoze to provision 

to on

![Hosted Zone](Hosted%20Zone.png)

AWS detailed instructions can be found here: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html

### Step 2. Create a Compoze IAM Role

For detail instructions to create an IAM role, refer to: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html.

   1. The trusted entity type should be "AWS Account"
   2. The role should be for Another AWS account with Account ID: "228304625426"
   3. Select the checkbox "Require external ID" and provide a UUID value
   4. Select and apply policy "AdministratorAccess" to the role
   5. The role should be named exactly as: "CompozeAutomationRole"

```json
{
   "name": "alec"
}
```

### Step 3. Enable non root accounts to interact with billing information

This is done via the root account login.