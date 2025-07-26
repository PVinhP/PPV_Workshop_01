---
title : "Create and Test Knowledge Base"
date :  2025-07-17
weight : 9
chapter : false
pre : " <b> 9.1 </b> "
---

Creating an Amazon Bedrock Knowledge Base involves integrating proprietary data into AI applications using Retrieval Augmented Generation (RAG). The process converts text from data sources into vector embeddings using models like Amazon Titan Embeddings, storing them in a vector database such as OpenSearch Serverless. Once set up, the Knowledge Base can be queried to retrieve relevant information, augmenting prompts for foundation models.

## Create Knowledge Base

1. Open [Amazon Bedrock console](https://console.aws.amazon.com/bedrock).
2. At the upper-left of the Amazon Bedrock console, choose the menu ☰ icon. In the navigation pane, choose **Knowledge Bases**.
3. Choose **Create knowledge base**.
4. Keep the default name.
5. In **IAM permissions**, choose **Use an existing role**.
6. From the dropdown, select `xxxx-BedrockExecutionRoleForKBs-xxxx` role.
7. Select **Amazon S3** for the data source.
8. Choose **Next**.
9. Choose **Browse S3**.
10. Choose `xxxx-s3bucket-xxxx` and **sample-documents** folder.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/000-architecture.png?raw=true)