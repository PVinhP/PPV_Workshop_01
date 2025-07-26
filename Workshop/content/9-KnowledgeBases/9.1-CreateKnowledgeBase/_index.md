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
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/001.png?raw=true)
3. Choose **Create knowledge base**, select **Knowledge base with vector store**
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/002.png?raw=true)
4. Keep the default name.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/003.png?raw=true)
5. In **IAM permissions**, choose **Use an existing role**.
6. From the dropdown, select `xxxx-BedrockExecutionRoleForKBs-xxxx` role.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/004.png?raw=true)
7. Select **Amazon S3** for the data source.
8. Choose **Next**.
9. Choose **Browse S3**.
10. Choose `xxxx-s3bucket-xxxx` and **sample-documents** folder.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/007.png?raw=true)
11. Keep the default chunking configuration.  
12. Choose **Next**.  
13. For **Embeddings model**, choose **Titan Text Embeddings v2**.  
14. For **Vector database**, choose **Quick create...**  
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/008.png?raw=true)
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/009.png?raw=true)
15. For **Vector store**, choose **Amazon OpenSearch Serverless**  
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/010.png?raw=true)
16. Choose **Create knowledge base**.  
17. This may take few minutes to complete.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/011.png?raw=true)
## Sync data source with Knowledge base

1. Under **Data source**, choose your knowledge source (*knowledge-base-quick-...*).  
2. Choose **Sync** button.  
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/014.png?raw=true)
3. After few seconds, the knowledge base is ready to test.
## Test Knowledge base
1. Select your Knowledge base and choose Test knowledge base button.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/015.png?raw=true)

2. In the right side pane, choose **Select model**.

3. In the **Select model** window, choose **Claude 3 Haiku** and choose **Apply**.  
   _(Note, you may test with other models also as long as you have enabled those models.)_
   
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/016.png?raw=true)

4. Type the below question in the _Enter your message here_ window.

    ```text
    What are Amazon sustainability goals by year 2040?
    ```

5. Choose **Run**.

6. You see a response shown below.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/017.png?raw=true)
Explore the capabilities of your knowledge base by posing diverse questions. A notable feature is its ability to maintain context throughout the conversation. This allows for progressive inquiry, where each question can build upon previous responses, enabling more in-depth and nuanced interactions. Take advantage of this contextual awareness to delve deeper into topics and extract more comprehensive insights from your data.
