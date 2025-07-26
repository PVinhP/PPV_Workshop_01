---
title : "Launch a CloudFormation stack"
date : 2025-06-17
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

{{% notice info %}}
**Supported Regions**: We recommended that you run this workshop in the **us-west-2** AWS Region.
{{% /notice %}} 

An AWS CloudFormation template is used to set up lab resources in the AWS Region that you choose. This step is required because later instructions are based on these resources. The CloudFormation template creates the following AWS resources:

- **VSCode**: VSCode on Amazon EC2 is a cloud-based integrated development environment (IDE) that you can use to write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. In this workshop, you use VSCode editor to deploy a backend application, which is built by using AWS Serverless Application Model (AWS SAM), and also deploy AWS Amplify frontend.
- **Amazon S3**: Amazon Simple Storage Service (Amazon S3) is an object storage service offering industry-leading scalability, data availability, security, and performance. Customers of all sizes and industries can store and protect any amount of data for virtually any use case, such as data lakes, cloud-centered applications, and mobile apps. In this workshop, you use an S3 bucket to upload your use case-centric documents. Amazon Kendra indexes these documents to provide Retrieval-Augmented Generation (RAG) answers to questions that you ask.
- **Amazon Kendra**: Amazon Kendra is an intelligent enterprise search service that helps you search across different content repositories with built-in connectors.
Download the CloudFormation template:

1. Download the CloudFormation template: [<span style="background-color:#f90; color:#000; padding:6px 14px; border-radius:10px;">Download</span>](https://static.us-east-1.prod.workshops.aws/public/c5c516a7-10ce-444b-a0c5-1e60794fdb7c/static/template/chatbot-startup-stack.yaml)
2. Store the YAML template file in a folder on your local machine.
3. Navigate to [**AWS CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home) 🔗
4. On the CloudFormation console, choose **Upload a template file**.
5. Select the template that you just downloaded, and then choose [<span style="background-color:#1b64da; color:#000; padding:6px 14px; border-radius:10px;">Next</span>](#)

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh_1.png?raw=true)

6. Give the stack a name, such as `chatbot-startup-stack`

7. Keep other values unchanged, and choose **`Next`**

8. For **Configure stack options**, select *I acknowledge...* options and choose **`Next`**

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh_2.png?raw=true)
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh_3.png?raw=true)

9. To deploy the template, choose **`Submit`**
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh_4.png?raw=true)

10. After the template is deployed, to review the created resources, navigate to [CloudFormation Resources](#), and then select the CloudFormation stack that you created.

> ⏳ *Template deployment takes 10–15 minutes to complete all AWS resource provisioning.*

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh5.png?raw=true)

**Congratulations! You can now proceed to the next task.**

---
