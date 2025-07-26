---
title : "Introduction"
date :  2025-06-17
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
### Overall
In this workshop, you will embark on a comprehensive journey to create a serverless chatbot using cutting-edge generative AI technologies. You'll leverage Amazon Bedrock, Amazon Kendra, and your own data to build an intelligent conversational interface that can retrieve and generate contextually relevant responses. The workshop will guide you through configuring multiple services including AWS Lambda, API Gateway, and AWS Amplify, and you'll work with various large language models like Claude 3, Mistral, and Llama. By the end of the workshop, you'll have a fully functional chatbot that can interact with enterprise knowledge bases using Retrieval-Augmented Generation (RAG) techniques. This workshop addresses real-world use cases where organizations need intelligent, data-driven conversational interfaces.

You'll learn how to solve challenges such as enterprise knowledge discovery, customer support automation, and information retrieval across complex document repositories. The solution you'll build can be applied in numerous business scenarios, including technical support systems, HR knowledge management, compliance document querying, and research information synthesis. By mastering these techniques, you'll be equipped to create AI-powered solutions that can transform how businesses interact with their data, providing faster, more accurate, and context-aware responses to complex queries.

Throughout this workshop, you have gained the following knowledge and skills:

- How to construct generative AI solutions by utilizing Amazon Bedrock and a serverless architecture.
- How to use AWS SAM to create and deploy generative AI solutions.
- How to harness the capabilities of Large Language Models through the Retrieval-Augmented Generation (RAG) architecture.
- The importance of effective data management in the context of generative AI, which includes sourcing, storing, and preprocessing data to enhance the performance and relevance of AI-generated responses.
- The role of Prompt Engineering in achieving better outcomes.

You will create the following architecture for this workshop:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/000-architecture.png?raw=true)
### 🧩 Components

- **Users**: End-users of the chatbot  
- **Web UI**: User interface for chatbot interaction  
- **AWS Amplify (React, Vue.js)**: Manages front-end and authentication, specifically using React or Vue.js  
- **Amazon API Gateway**: Handles API requests  
- **AWS Lambda (RAG/KB/LLM Functions)**: Executes serverless functions for Retrieval-Augmented Generation, Knowledge Base operations, and LLM interactions  
- **Amazon Bedrock**: Provides access to AI models and services  
- **Large Language Models (Claude 3, Mistral, Llama etc.)**: AI models powering responses  
- **Knowledge Bases**: Stores structured information  
- **Amazon S3**: Object storage for documents and data  
- **Documents (PDF, CSV, TXT etc.)**: Various file types for ingestion  
- **Amazon Kendra**: Intelligent search service  
- **Amazon OpenSearch**: Vector database and search engine for efficient similarity search  
- **Amazon Cognito**: User authentication and authorization  

---

### 🔄 Workflow

1. Users interact with the Web UI  
2. Requests routed through API Gateway to Lambda functions  
3. Lambda functions use Bedrock for LLM access, RAG operations, and Knowledge Base interactions  
4. Knowledge retrieved from Knowledge Bases, S3, Kendra, or OpenSearch  
5. System incorporates various document types to enhance the knowledge base  

---

### ✨ Key Features

- Scalable, serverless architecture  
- Leverages various LLM models including Claude 3, Mistral, and Llama  
- Incorporates enterprise knowledge through Kendra and custom knowledge bases  
- Secure authentication with Cognito  
- Flexible document ingestion and search capabilities  
- Front-end built with modern frameworks (React or Vue.js) using AWS Amplify  

---

This architecture enables a **generative AI-powered chatbot solution** that can scale with demand while providing **intelligent responses** based on both **LLMs' knowledge and enterprise-specific knowledge**. The use of **React or Vue.js with AWS Amplify** ensures a responsive and efficient user interface.


### Amazon Kendra

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/Kendra.png?raw=true)

**Amazon Kendra** is a managed information retrieval and intelligent search service that uses natural language processing and advanced deep learning model. Unlike traditional keyword-based search, Amazon Kendra uses semantic and contextual similarity—and ranking capabilities—to decide whether a text chunk or document is relevant to a retrieval query.

Source: [Amazon Kendra – What is Kendra?](https://docs.aws.amazon.com/kendra/latest/dg/what-is-kendra.html)  
  *This source provides a more detailed explanation of how Amazon Kendra works.*

### Amazon Bedrock

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/Bedrock.png?raw=true)
**Amazon Bedrock** is a fully managed service that makes high-performing foundation models (FMs) from leading AI companies and Amazon available for your use through a unified API. You can choose from a wide range of foundation models to find the model that is best suited for your use case. Amazon Bedrock also offers a broad set of capabilities to build generative AI applications with security, privacy, and responsible AI. Using Amazon Bedrock, you can easily experiment with and evaluate top foundation models for your use cases, privately customize them with your data using techniques such as fine-tuning and Retrieval Augmented Generation (RAG), and build agents that execute tasks using your enterprise systems and data sources.

With Amazon Bedrock's serverless experience, you can get started quickly, privately customize foundation models with your own data, and easily and securely integrate and deploy them into your applications using AWS tools without having to manage any infrastructure.

Source: [Amazon Bedrock – What is Amazon Bedrock?](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)  
  *This source provides a more detailed explanation of how Amazon Bedrock works.*