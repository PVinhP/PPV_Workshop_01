---
title : "Clean up resources"
date :  2025-07-17
weight : 10
chapter : false
pre : " <b> 10. </b> "
---

After you've completed the workshop, to stop incurring costs, you should remove the resources that you created in your AWS account.

## Remove the AWS SAM application and startup CloudFormation stack

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