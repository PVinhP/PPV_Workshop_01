---
title : "Rag Chat with Bedrock Knowledge Bases"
date :  2025-07-17
weight : 9
chapter : false
pre : " <b> 9. </b> "
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

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/003.png?raw=true)

## Index the sample documents by using Amazon Kendra

The Amazon Kendra index and Amazon S3 data source were created during the initial provisioning for this workshop. In this task, you index all the documents in the S3 data source.
1. [Open Amazon Kendra console](https://console.aws.amazon.com/kendra/)

2. At the upper-left of the Amazon Kendra console, choose the menu menu ☰ icon, and then, in the navigation pane, choose Indexes.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/001.png?raw=true)



![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/002.png?raw=true)

3. Click on the index name to see the left navigation pane.

4. To start indexing all the documents from the sample-documents folder, select the S3DocsDataSource, and then choose Sync now. The indexing might take a couple minutes. Wait for it to be completed.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/004.png?raw=true)
5. To query the Amazon Kendra index with a few sample questions, in the left navigation pane, choose Search indexed content, and then ask a question.

Sample question:
````bash
What is federal funds rate as of May 2024??
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/005.png?raw=true)

--- 
### Congratulations
You can now proceed to next task.