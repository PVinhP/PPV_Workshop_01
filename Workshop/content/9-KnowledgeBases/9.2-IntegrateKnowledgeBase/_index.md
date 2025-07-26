---
title : "Integrate Knowledge Base with a Lambda Function"
date :  2025-07-17
weight : 9
chapter : false
pre : " <b> 9.2 </b> "
---

In this task, you will integrate the Amazon Bedrock Knowledge Base with an AWS Lambda function, utilizing the existing API Gateway setup.  
The focus will be on setting up a Lambda function that interacts with the Knowledge Base and connects to the pre-established API endpoint.  
This integration will enable the existing UI to communicate with the Knowledge Base through the serverless architecture, allowing for efficient querying and retrieval information with RAG.  
By leveraging the pre-configured API Gateway and implementing the Lambda function, and complete the backend infrastructure, enabling users to access the power of the Knowledge Base through the familiar web interface, while maintaining the scalability and flexibility of the cloud-native solution.

---

## ⚙️ Setting up AWS Lambda Function

The Lambda function was previously deployed as part of the SAM deployment in Task 2.  
In this task, you will update the function's code and an environment variable containing the Knowledge Base ID.  
These changes will enable the Lambda function to interact effectively with the Knowledge Base.

> ℹ️ **Note**  
> Before proceeding, ensure the knowledge base creation is complete.  
> This step requires the knowledge base ID, which will be used as a Lambda environment variable.  
> Verify the status of the knowledge base you created in the previous task, and only continue once it's fully operational.

---

1. Open the **VSCode** editor.  
2. From the **`bedrock-serverless-workshop`** project, open  
   **`/lambdas/llmFunctions/kbfunction.py`**, copy the below code and update the function code.  
   This function carries the logic to call knowledge base.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/018.png?raw=true)

````bash
import os
import json
import boto3

import traceback


region = boto3.session.Session().region_name
KB_ID = os.environ["KB_ID"]


def lambda_handler(event, context):
    boto3_version = boto3.__version__
    print(f"Boto3 version: {boto3_version}")
    
    print(f"Event is: {event}")
    event_body = json.loads(event["body"])
    prompt = event_body["query"]
    model_id = event_body["model_id"]
    
    response = ''
    status_code = 200
    
    try:
        model_arn = 'arn:aws:bedrock:'+region+'::foundation-model/'+model_id
        print(f"Model arn: {model_arn}")
        
        response = retrieveAndGenerate(prompt, model_arn)["output"]["text"]
        return {
            'statusCode': status_code,
            'headers': {
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Headers': 'Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token',
                'Access-Control-Allow-Methods': 'OPTIONS,POST'
            },
            'body': json.dumps({'answer': response})
        }
            
    except Exception as e:
        print(f"An unexpected error occurred: {str(e)}")
        stack_trace = traceback.format_exc()
        print(stack_trace)
        return {
            'statusCode': status_code,
            'headers': {
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Headers': 'Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token',
                'Access-Control-Allow-Methods': 'OPTIONS,POST'
            },
            'body': json.dumps({'error': str(e)})
        }

def retrieveAndGenerate(prompt, model_arn):
    bedrock_agent_runtime = boto3.client(
            service_name = "bedrock-agent-runtime")
    return bedrock_agent_runtime.retrieve_and_generate(
        input={
            'text': prompt
        },
        retrieveAndGenerateConfiguration={
            'type': 'KNOWLEDGE_BASE',
            'knowledgeBaseConfiguration': {
                'knowledgeBaseId': KB_ID,
                'modelArn': model_arn
                }
            }
    )
````
1. Run the following commands to retrieve the Knowledge Base ID and update the Lambda function's environment variable:
````bash
export KB_ID=$(aws bedrock-agent list-knowledge-bases | jq -r '.knowledgeBaseSummaries[0].knowledgeBaseId')
echo "Knowledge Base ID: $KB_ID"
sed -Ei "s|copy_kb_id|${KB_ID}|g" ./template.yaml
````
2. Open VSCode terminal, run the following command to build and deploy with new updated lambda code.

````bash
cd ~/environment/bedrock-serverless-workshop
sam build && sam deploy
````

The Lambda function has been successfully integrated with your Amazon Bedrock Knowledge Base.  
This integration enables the Lambda function to interact directly with the Knowledge Base, allowing it to retrieve and process information from your proprietary data.

---

## 🧪 Test Knowledge Base using UI

1. Return to the browser and open the chatbot webpage.  
   If you are not signed in, use the credentials that you retrieved earlier to sign in.

2. Choose **RAG with Knowledge Bases** option from the menu.

3. Ask a question — you can use a sample question from the right side panel.  
   Here is a sample question and the response from **Claude 3.5 Sonnet**.  
   You can try with other LLMs and compare the results.

```text
What are Amazon sustainability goals by year 2040?
```

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/019.png?raw=true)

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/020.png?raw=true)
This task has guided you through the process of creating and integrating an Amazon Bedrock Knowledge Base with AWS Lambda and API Gateway. You've successfully set up serverless architecture that leverages your organization's proprietary data to enhance AI-driven applications. By connecting your Knowledge Base to Lambda and utilizing the existing API Gateway, you've created a robust backend capable of processing queries, retrieving relevant information, and generating context-aware responses. This integration opens up numerous possibilities for developing intelligent applications, from advanced chatbots to sophisticated search interfaces, all while maintaining data security and scalability.
