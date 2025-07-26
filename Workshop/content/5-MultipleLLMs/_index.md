---
title : "General Chat with Multiple LLMs"
date :  2025-07-17
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

{{% notice info %}}
**Port Forwarding** is a useful way to redirect network traffic from one IP address - Port to another IP address - Port. With **Port Forwarding** we can access an EC2 instance located in the private subnet from our workstation.
{{% /notice %}}

You have successfully deployed the Amazon Bedrock serverless chatbot application, using AWS SAM for the backend and AWS Amplify for the frontend. The frontend serves as a basic user interface for testing the solution with various questions and prompt parameters. In this section, you will update and deploy a LLM Lambda function, this function enables with a general chat with multiple large language models.

1. Open the VSCode editor (as discussed in Task 1).
2. From bedrock-serverless-workshop project, open /lambdas/llmFunctions/llmfunction.py function, copy the below code and update the function code. This function carries the logic to support Claud3 (Haiku, Sonnet etc.), Mistral and Llama models.

````bash
import boto3
import json
import traceback


region = boto3.session.Session().region_name

def lambda_handler(event, context):
    boto3_version = boto3.__version__
    print(f"Boto3 version: {boto3_version}")
    
    print(f"Event is: {event}")
    event_body = json.loads(event["body"])
    prompt = event_body["query"]
    temperature = event_body["temperature"]
    max_tokens = event_body["max_tokens"]
    model_id = event_body["model_id"]
    
    response = ''
    status_code = 200
    
    try:
        if model_id == 'mistral.mistral-7b-instruct-v0:2':
            response = invoke_mistral_7b(model_id, prompt, temperature, max_tokens)
        elif model_id == 'meta.llama3-1-8b-instruct-v1:0':
            response = invoke_llama(model_id, prompt, temperature, max_tokens)
        else:
            response = invoke_claude(model_id, prompt, temperature, max_tokens)
            
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


def invoke_claude(model_id, prompt, temperature, max_tokens):
    try:

        instruction = f"Human: {prompt} nAssistant:"
        bedrock_runtime_client = boto3.client(service_name="bedrock-runtime", region_name=region)
        body= {
                "anthropic_version": "bedrock-2023-05-31",
                "max_tokens": max_tokens,
                "messages": [
                    {
                        "role": "user",
                        "content": [{"type": "text", "text": prompt}],
                    }
                ],
        }

        response = bedrock_runtime_client.invoke_model(
            modelId=model_id, body=json.dumps(body)
        )

        response_body = json.loads(response["body"].read())
        outputs = response_body.get("content")
        completions = [output["text"] for output in outputs]
        print(f"completions: {completions[0]}")

        return completions[0]

    except Exception as e:
        raise
        
def invoke_mistral_7b(model_id, prompt, temperature, max_tokens):
    try:
        instruction = f"<s>[INST] {prompt} [/INST]"
        bedrock_runtime_client = boto3.client(service_name="bedrock-runtime", region_name=region)

        body = {
            "prompt": instruction,
            "max_tokens": max_tokens,
            "temperature": temperature,
        }

        response = bedrock_runtime_client.invoke_model(
            modelId=model_id, body=json.dumps(body)
        )
        response_body = json.loads(response["body"].read())
        outputs = response_body.get("outputs")
        print(f"response: {outputs}")

        completions = [output["text"] for output in outputs]
        return completions[0]
    except Exception as e:
        raise
        
def invoke_llama(model_id, prompt, temperature, max_tokens):
    print(f"Invoking llam model {model_id}" )
    print(f"max_tokens {max_tokens}" )
    try:
        instruction = f"[INST]You are a very intelligent bot with exceptional critical thinking, help me answering below question.[/INST]"
        total_prompt = f"{instruction}\n{prompt}" 
        
        print(f"Prompt template {total_prompt}" )

        bedrock_runtime_client = boto3.client(service_name="bedrock-runtime", region_name=region)
        
        body = {
            "prompt": total_prompt,
            "max_gen_len": max_tokens,
            "temperature": temperature,
            "top_p": 0.9
        }

        response = bedrock_runtime_client.invoke_model(
            modelId=model_id, body=json.dumps(body)
        )
        response_body = json.loads(response["body"].read())
        print(f"response: {response_body}")
        return response_body ['generation']
    except Exception as e:
        raise

````

3. From VSCode editor, run the following command to build and deploy with new updated lambda code.

````bash
cd ~/environment/bedrock-serverless-workshop
sam build && sam deploy
````
You can inspect the updated Lambda function using [AWS Lambda Console](https://console.aws.amazon.com/lambda/).


In this task, you have the opportunity to work with following LLM models, each offering unique functionalities and specializations. These models include:

- **anthropic.claude-3-haiku** - Claude 3 Haiku is Anthropic’s fastest, most compact model for near-instant responsiveness. Haiku is the best choice for building seamless AI experiences that mimic human interactions.
- **anthropic.claude-3-5-sonnet** - Anthropic’s most intelligent and advanced model, Claude 3.5 Sonnet, demonstrates exceptional capabilities across a diverse range of tasks and evaluation.
- **anthropic.claude-3-opus** - Opus is a highly intelligent model with reliable performance on complex tasks. It can navigate open-ended prompts and sight-unseen scenarios with remarkable fluency and human-like understanding. Use Opus to automate tasks, and accelerate research and development across a diverse range of use cases and industries.
- **mistral.mistral-7b-instruct - Mistral** is a highly efficient large language model optimized for high-volume, low-latency language-based tasks. Popular use cases for Mistral are text summarization, structuration, question answering, and code completion
- **meta.llama3-1-8b-instruct - Llama 3.1 8B** is best suited for limited computational power and resources. The model excels at text summarization, text classification, sentiment analysis, and language translation requiring low-latency inferencing.


## Test using UI
1. Return to the browser and open the chatbot webpage. If you are not signed in, use the credentials that you retrieved earlier to sign in. The webpage is displayed as follows:
