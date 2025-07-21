---
title : "Preparation "
date : 2025-06-17
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
If you have access to the AWS Management Console with administrative privileges, you can use your AWS account to initiate this workshop.
{{% /notice %}}

In this workshop, you configure the following AWS services to build a generative AI chatbot.
  - [Amazon Bedrock](https://aws.amazon.com/vi/bedrock/)
  - [Knowledge Bases ](https://aws.amazon.com/vi/bedrock/knowledge-bases/)
  - [Amazon Kendra](https://aws.amazon.com/vi/kendra/)
  - [AWS Lambda](https://aws.amazon.com/vi/lambda/)

An AWS CloudFormation template is used to set up lab resources in the AWS Region that you choose. This step is required because later instructions are based on these resources
**Supported Regions**: We recommended that you run this workshop in the **us-west-1** AWS Region.
{{% notice warning %}}
**Cost**: Be aware that if you run this workshop in your own AWS account, you will incur costs for resource usage. The Clean Up section in the left navigation pane can help you remove resources from your environment when you are done.
{{% /notice %}}
### Content
  - [Launch a CloudFormation stack](2.1-launchastack/)
  - [Create IAM Role](2.2-createiamrole/)