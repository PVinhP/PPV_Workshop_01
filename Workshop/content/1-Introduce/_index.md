---
title : "Introduction"
date :  2025-06-17
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
## Overall
In this workshop, you will embark on a comprehensive journey to create a serverless chatbot using cutting-edge generative AI technologies. You'll leverage Amazon Bedrock, Amazon Kendra, and your own data to build an intelligent conversational interface that can retrieve and generate contextually relevant responses. The workshop will guide you through configuring multiple services including AWS Lambda, API Gateway, and AWS Amplify, and you'll work with various large language models like Claude 3, Mistral, and Llama. By the end of the workshop, you'll have a fully functional chatbot that can interact with enterprise knowledge bases using Retrieval-Augmented Generation (RAG) techniques. This workshop addresses real-world use cases where organizations need intelligent, data-driven conversational interfaces.

You'll learn how to solve challenges such as enterprise knowledge discovery, customer support automation, and information retrieval across complex document repositories. The solution you'll build can be applied in numerous business scenarios, including technical support systems, HR knowledge management, compliance document querying, and research information synthesis. By mastering these techniques, you'll be equipped to create AI-powered solutions that can transform how businesses interact with their data, providing faster, more accurate, and context-aware responses to complex queries.

You will create the following architecture for this workshop:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/000-architecture.png?raw=true)

### Amazon Kendra

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/Kendra.png?raw=true)

Amazon Kendra is a managed information retrieval and intelligent search service that uses natural language processing and advanced deep learning model. Unlike traditional keyword-based search, Amazon Kendra uses semantic and contextual similarity—and ranking capabilities—to decide whether a text chunk or document is relevant to a retrieval query.

Source: [Amazon Kendra – What is Kendra?](https://docs.aws.amazon.com/kendra/latest/dg/what-is-kendra.html)  
  *This source provides a more detailed explanation of how Amazon Kendra works.*

### Amazon Bedrock


Amazon Bedrock is a fully managed service that makes high-performing foundation models (FMs) from leading AI companies and Amazon available for your use through a unified API. You can choose from a wide range of foundation models to find the model that is best suited for your use case. Amazon Bedrock also offers a broad set of capabilities to build generative AI applications with security, privacy, and responsible AI. Using Amazon Bedrock, you can easily experiment with and evaluate top foundation models for your use cases, privately customize them with your data using techniques such as fine-tuning and Retrieval Augmented Generation (RAG), and build agents that execute tasks using your enterprise systems and data sources.

With Amazon Bedrock's serverless experience, you can get started quickly, privately customize foundation models with your own data, and easily and securely integrate and deploy them into your applications using AWS tools without having to manage any infrastructure.

**Session Manager** 