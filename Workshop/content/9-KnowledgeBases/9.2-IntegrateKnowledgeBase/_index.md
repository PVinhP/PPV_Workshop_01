---
title : "Integrate Knowledge Base with a Lambda Function"
date :  2025-07-17
weight : 9
chapter : false
pre : " <b> 9.2 </b> "
---

Amazon Bedrock Knowledge Bases is a fully managed capability that enables you to implement Retrieval Augmented Generation (RAG) workflows using your organization's proprietary data sources. Amazon Bedrock Knowledge Bases automates the entire RAG workflow, from data ingestion to retrieval and prompt augmentation, without requiring custom integrations or data flow management. This allows you to equip foundation models (FMs) and agents with up-to-date and proprietary information to deliver more relevant and accurate responses.

Amazon Bedrock Knowledge Bases rely on two key components to enable efficient retrieval of relevant information: embeddings and vector stores. These elements work together to transform text data into a format that can be quickly searched and retrieved based on semantic similarity.

Embeddings are numerical representations of text that capture semantic meaning. In Amazon Bedrock, embedding models like Amazon Titan Embeddings or Cohere Embed convert text from your documents into dense vectors. This process allows for efficient comparison and retrieval of semantically similar content, forming the foundation of the knowledge base's understanding of your data.

Vector stores are specialized databases designed to index and query these vector embeddings efficiently. Amazon Bedrock offers options like Amazon-managed OpenSearch Serverless or custom solutions such as Amazon Aurora PostgreSQL with pgvector. These stores enable rapid similarity searches, allowing the system to quickly identify and retrieve the most relevant information when responding to queries, thus enhancing the performance of Retrieval Augmented Generation (RAG) workflows.
## Functionality and Benefits
Seamless RAG Implementation: Amazon Bedrock Knowledge Bases automates the entire RAG workflow, from data ingestion to retrieval and prompt augmentation, without requiring custom integrations or data flow management. This allows you to equip foundation models (FMs) and agents with up-to-date and proprietary information to deliver more relevant and accurate responses.

Secure Data Connection: The service automatically fetches documents from your specified data sources, including Amazon S3, Web Crawler, Salesforce, and SharePoint. It then processes the content by dividing it into text blocks, converting them into embeddings, and storing them in a vector database.

Customization Options: You can fine-tune both retrieval and ingestion processes to improve accuracy across different use cases. Advanced parsing options are available for understanding complex unstructured data, and you can choose from various chunking strategies or even write custom chunking code.
## Knowledge Bases Architecture
In the next two tasks, you will build a chatbot using Amazon Bedrock Knowledge Bases. The architecture of the solution is illustrated below:
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/000-architecture.png?raw=true)
## Knowledge Base and Vector Search Components

1. **Knowledge Bases**: Central repository for structured information  
2. **Amazon S3**: Storage for documents (pdf, csv, txt, etc.)  
3. **Amazon OpenSearch**: Vector database and search engine for efficient similarity search  
4. **Amazon Bedrock**: Provides access to embedding models, including Amazon Titan Embeddings  

---

## Knowledge Base and Vector Search Workflow

1. Documents are stored in Amazon S3  
2. Text is extracted and processed from documents  
3. Amazon Bedrock's Titan Embeddings model generates vector representations of text  
4. Vectors are stored in Amazon OpenSearch, functioning as a vector database  
5. Knowledge Bases ingest and structure information, incorporating vector representations  
6. AWS Lambda functions (RAG/KB/LLM Functions) interact with Knowledge Bases and vector search  
7. RAG (Retrieval Augmented Generation) leverages vector similarity search for relevant information retrieval
## 🔍 Key Features of Knowledge Base and Vector Search Integration

- Semantic search capabilities using vector representations of text  
- Efficient similarity search through Amazon OpenSearch's vector database functionality  
- Integration of Amazon Titan Embeddings for high-quality text vectorization  
- Enhanced context and accuracy in chatbot responses using vector-based retrieval  
- Improved relevance in information retrieval for RAG operations  
- Ability to handle and search through large volumes of unstructured text data  
- Seamless combination of traditional keyword search and vector-based semantic search  

---

This setup allows the chatbot to perform advanced semantic searches on enterprise knowledge.  
By using Amazon Titan Embeddings to create vector representations of text and storing these in Amazon OpenSearch as a vector database, the system can find contextually similar information even when exact keyword matches are not present.  
This significantly enhances the chatbot's ability to understand and respond to user queries with relevant information from the enterprise's knowledge base.
