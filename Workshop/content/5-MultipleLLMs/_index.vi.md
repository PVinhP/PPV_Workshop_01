---
title : "Trò chuyện tổng quát với nhiều mô hình LLM"
date : 2025-07-17
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

{{% notice info %}}
**Port Forwarding** (chuyển tiếp cổng) là một cách hữu ích để chuyển hướng lưu lượng mạng từ một địa chỉ IP - Cổng này sang một địa chỉ IP - Cổng khác. Với **Port Forwarding**, chúng ta có thể truy cập một EC2 instance nằm trong private subnet từ máy làm việc cá nhân (workstation).
{{% /notice %}}

Bạn đã triển khai thành công ứng dụng chatbot sử dụng Amazon Bedrock với kiến trúc serverless, trong đó backend sử dụng AWS SAM và frontend sử dụng AWS Amplify. Giao diện frontend đóng vai trò là giao diện người dùng cơ bản để kiểm thử giải pháp với các câu hỏi và tham số prompt khác nhau. Trong phần này, bạn sẽ cập nhật và triển khai một hàm Lambda hỗ trợ trò chuyện tổng quát với nhiều mô hình ngôn ngữ lớn (LLM).

1. Mở trình soạn thảo VSCode (như đã hướng dẫn trong Nhiệm vụ 1).  
2. Trong dự án `bedrock-serverless-workshop`, mở file `/lambdas/llmFunctions/llmfunction.py`, sao chép đoạn mã dưới đây và cập nhật nội dung của hàm. Hàm này chứa logic hỗ trợ các mô hình Claude3 (Haiku, Sonnet, v.v.), Mistral và Llama.

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

3. Từ trình soạn thảo VSCode, chạy lệnh sau để build và triển khai mã Lambda đã được cập nhật:

````bash
cd ~/environment/bedrock-serverless-workshop
sam build && sam deploy
````
Bạn có thể kiểm tra lại hàm Lambda đã cập nhật thông qua [Bảng điều khiển AWS Lambda](https://console.aws.amazon.com/lambda/).

Trong nhiệm vụ này, bạn sẽ có cơ hội làm việc với các mô hình LLM sau, mỗi mô hình mang lại các tính năng và điểm mạnh riêng biệt:

- **anthropic.claude-3-haiku** – Claude 3 Haiku là mô hình nhanh nhất và nhỏ gọn nhất của Anthropic, mang lại khả năng phản hồi gần như tức thì. Đây là lựa chọn lý tưởng để xây dựng trải nghiệm AI mượt mà, mô phỏng tương tác như con người.
- **anthropic.claude-3-5-sonnet** – Claude 3.5 Sonnet là mô hình tiên tiến và thông minh nhất của Anthropic, thể hiện năng lực vượt trội trong nhiều loại tác vụ và đánh giá khác nhau.
- **anthropic.claude-3-opus** – Opus là một mô hình cực kỳ thông minh với hiệu suất đáng tin cậy cho các tác vụ phức tạp. Nó có thể xử lý các prompt mở và tình huống chưa từng thấy với độ lưu loát và hiểu biết giống con người. Sử dụng Opus để tự động hóa, thúc đẩy nghiên cứu và phát triển trong nhiều ngành nghề và trường hợp sử dụng.
- **mistral.mistral-7b-instruct** – Mistral là một mô hình ngôn ngữ lớn rất hiệu quả, được tối ưu hóa cho các tác vụ ngôn ngữ khối lượng lớn với độ trễ thấp. Các trường hợp sử dụng phổ biến của Mistral gồm: tóm tắt văn bản, cấu trúc hóa nội dung, trả lời câu hỏi, và hoàn thành mã nguồn.
- **meta.llama3-1-8b-instruct** – Llama 3.1 8B phù hợp với môi trường giới hạn tài nguyên tính toán. Mô hình này đặc biệt mạnh ở các tác vụ như tóm tắt văn bản, phân loại văn bản, phân tích cảm xúc, và dịch ngôn ngữ yêu cầu suy luận độ trễ thấp.

---

## Kiểm thử qua giao diện người dùng (UI)

1. Quay lại trình duyệt và mở trang web chatbot. Nếu bạn chưa đăng nhập, hãy sử dụng thông tin đăng nhập đã lấy trước đó để đăng nhập. Giao diện trang web sẽ hiển thị như sau:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/001.png?raw=true)

2. Nhập bất kỳ câu hỏi nào vào ô Query và nhấn nút **Ask Question**. Bạn cũng có thể thử một số câu hỏi mẫu từ khung bên phải. Dưới đây là ví dụ một câu hỏi và phản hồi từ Claude 3 Haiku. Bạn có thể thử với các mô hình LLM khác và so sánh kết quả.
````bash
List top 10 cities of VietNam by population?
````
---

### Hoàn thành nhiệm vụ

Bạn đã sẵn sàng để tiếp tục sang nhiệm vụ tiếp theo.