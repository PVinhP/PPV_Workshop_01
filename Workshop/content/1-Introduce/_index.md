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


Amazon Kendra is a managed information retrieval and intelligent search service that uses natural language processing and advanced deep learning model. Unlike traditional keyword-based search, Amazon Kendra uses semantic and contextual similarity—and ranking capabilities—to decide whether a text chunk or document is relevant to a retrieval query.

Source: [Amazon Kendra – What is Kendra?](https://docs.aws.amazon.com/kendra/latest/dg/what-is-kendra.html)  
  *This source provides a more detailed explanation of how Amazon Kendra works.*

**Session Manager** 