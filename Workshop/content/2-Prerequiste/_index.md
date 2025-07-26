---
title : "Preparation "
date : 2025-06-17
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
 It is recommended to use an IAM user account with administrative privileges rather than the root account. Some AWS services, such as Knowledge Bases in Amazon Bedrock or other AI-related services, may be restricted or unavailable when using the root account. For security and compatibility reasons, AWS best practices also advise avoiding the use of the root user for daily tasks.
{{% /notice %}}
{{% notice info %}}
**Supported Regions**: We recommended that you run this workshop in the **us-west-2** AWS Region.
{{% /notice %}} 
In this workshop, you configure the following AWS services to build a generative AI chatbot.
  - [Amazon Bedrock](https://aws.amazon.com/vi/bedrock/)
  - [Knowledge Bases ](https://aws.amazon.com/vi/bedrock/knowledge-bases/)
  - [Amazon Kendra](https://aws.amazon.com/vi/kendra/)
  - [AWS Lambda](https://aws.amazon.com/vi/lambda/)

An AWS CloudFormation template is used to set up lab resources in the AWS Region that you choose. This step is required because later instructions are based on these resources

{{% notice warning %}}
**Cost**: Be aware that if you run this workshop in your own AWS account, you will incur costs for resource usage. The Clean Up section in the left navigation pane can help you remove resources from your environment when you are done.
{{% /notice %}}
### Content
  - [2.1 Launch a CloudFormation stack](2.1-launchastack/)
  - [2.2 Launch VSCode on AWS, and Set Up the Environment](2.2-launchVScode/)