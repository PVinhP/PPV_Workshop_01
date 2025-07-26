---
title : "Clean up resources"
date :  2025-07-17
weight : 10
chapter : false
pre : " <b> 10. </b> "
---

After you've completed the workshop, to stop incurring costs, you should remove the resources that you created in your AWS account.

## Method 1: Use AWS CLI to delete resources

### Remove the AWS SAM application and startup CloudFormation stack

Navigate to the AWS Cloud9 terminal, and then run the following commands:

 1. To delete SAM stack, run the following command.
```bash
cd ~/environment/bedrock-serverless-workshop
sam delete
```
2. To delete startup stack, run the following command.
```bash
aws cloudformation delete-stack --stack-name $CFNStackName
```
## Method 2: Manually delete resources from the AWS Console
### Delete OpenSearch Collection
1. Go to Amazon OpenSearch Service in the AWS Console.

2. Click Collections under Serverless in the sidebar.

3. Select the collection → click Delete → confirm.
### Delete CloudFormation Stack
1. Go to AWS CloudFormation in the AWS Console.

2. Find the stack named **chatbot-startup-stack**.

3. Select it → click Delete → confirm.
### Delete a Knowledge Base
