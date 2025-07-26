---
title : "Index the Source Data by Using Amazon Kendra"
date :  2025-07-17
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

In the RAG architecture, Amazon Kendra can be leveraged to index and search through a collection of sample documents stored in Amazon S3 and other sources. Users can provide a query or question, and Kendra will perform a similarity search across the indexed content to identify the most relevant information.

Kendra's advanced natural language processing capabilities allow it to understand the user's intent and query, and then retrieve the most pertinent content from the indexed data sources. This enables users to quickly find the information they need, without having to manually sift through large volumes of documents.

By integrating Kendra into the "Retrieve" phase of the RAG architecture, organizations can enhance their overall search and information discovery capabilities, ultimately supporting more effective analysis and generation of insights and responses. Kendra's seamless integration with Amazon S3 simplifies the process of indexing and managing the underlying content, making it a powerful tool within the RAG framework.
## Download sample documents
Download a few sample documents to test this solution. The first document pertains to the May and September 2024 meeting minutes of the Federal Open Market Committee (FOMC). The second document is the 2023 Amazon Sustainability Report. The third document is the 2023 10K report for Henry Schein, a provider of dental services. You can download or use any document to test this solution, or you can bring your own data to conduct tests.

To copy the prompt templates and sample documents that you downloaded to the S3 bucket, run the following command.

Open the VSCode editor, then run the command in the **TERMINAL**.

````bash
cd ~/environment/bedrock-serverless-workshop
mkdir sample-documents

curl https://www.federalreserve.gov/monetarypolicy/files/monetary20240501a1.pdf --output sample-documents/monetary20240501a1.pdf
curl https://www.federalreserve.gov/monetarypolicy/files/monetary20240918a1.pdf --output sample-documents/monetary20240918a1.pdf
curl https://sustainability.aboutamazon.com/content/dam/sustainability-marketing-site/pdfs/reports-docs/2023-amazon-sustainability-report.pdf --output sample-documents/2023-sustainability-report-amazon.pdf
curl https://investor.henryschein.com/static-files/bcc116aa-a576-4756-a722-90f5e2e22114 --output sample-documents/2023-hs1-10k.pdf

````

## Upload sample documents and prompt templates
To copy prompt templates and sample documents you downloaded to the S3 bucket, run the following command.

````bash
cd ~/environment/bedrock-serverless-workshop
aws s3 cp sample-documents s3://$S3BucketName/sample-documents/ --recursive
aws s3 cp prompt-engineering s3://$S3BucketName/prompt-engineering/ --recursive
````
After a successful upload, review the Amazon S3 console and open the bucket. You should see something like this: