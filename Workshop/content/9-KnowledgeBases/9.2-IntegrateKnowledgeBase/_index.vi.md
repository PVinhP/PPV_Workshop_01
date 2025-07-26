---
title : "Tích hợp Knowledge Base với Lambda Function"
date : 2025-07-17
weight : 9
chapter : false
pre : " <b> 9.2 </b> "
---

Trong tác vụ này, bạn sẽ tích hợp Amazon Bedrock Knowledge Base với một AWS Lambda function, sử dụng thiết lập API Gateway hiện có.  
Mục tiêu là thiết lập một Lambda function có thể tương tác với Knowledge Base và kết nối tới endpoint API đã được cấu hình trước.  
Việc tích hợp này cho phép giao diện người dùng (UI) hiện có có thể giao tiếp với Knowledge Base thông qua kiến trúc serverless, giúp truy vấn và lấy thông tin hiệu quả bằng kỹ thuật RAG.  
Bằng cách tận dụng API Gateway đã được cấu hình và triển khai Lambda function, bạn sẽ hoàn thiện phần backend, cho phép người dùng truy cập vào sức mạnh của Knowledge Base thông qua giao diện web quen thuộc, đồng thời duy trì khả năng mở rộng và tính linh hoạt của giải pháp cloud-native.

---

## ⚙️ Thiết lập AWS Lambda Function

Lambda function đã được triển khai trước đó trong quá trình triển khai SAM ở Bước 2.  
Trong bước này, bạn sẽ cập nhật mã nguồn cho function và thêm biến môi trường chứa Knowledge Base ID.  
Các thay đổi này cho phép Lambda function tương tác hiệu quả với Knowledge Base.

> ℹ️ **Note**  
> Trước khi tiếp tục, đảm bảo rằng quá trình tạo Knowledge Base đã hoàn tất.  
> Bước này yêu cầu sử dụng Knowledge Base ID như một biến môi trường cho Lambda.  
> Hãy xác minh trạng thái của Knowledge Base bạn đã tạo ở bước trước và chỉ tiếp tục khi nó đã hoạt động đầy đủ.

---

1. Mở trình soạn thảo **VSCode**.  
2. Trong thư mục dự án **`bedrock-serverless-workshop`**, mở file **`/lambdas/llmFunctions/kbfunction.py`**, sao chép đoạn mã dưới đây và cập nhật vào file. Function này chứa logic gọi tới Knowledge Base.
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
3. Chạy các lệnh sau để truy xuất Knowledge Base ID và cập nhật biến môi trường của hàm Lambda:
````bash
export KB_ID=$(aws bedrock-agent list-knowledge-bases | jq -r '.knowledgeBaseSummaries[0].knowledgeBaseId')
echo "Knowledge Base ID: $KB_ID"
sed -Ei "s|copy_kb_id|${KB_ID}|g" ./template.yaml
````
4. Mở terminal trong VSCode, chạy lệnh sau để biên dịch và triển khai với mã Lambda đã được cập nhật:

````bash
cd ~/environment/bedrock-serverless-workshop
sam build && sam deploy
````

Hàm Lambda đã được tích hợp thành công với Amazon Bedrock Knowledge Base của bạn.
Việc tích hợp này cho phép hàm Lambda tương tác trực tiếp với Knowledge Base, giúp truy xuất và xử lý thông tin từ dữ liệu độc quyền của bạn.

---

## 🧪 Kiểm tra Knowledge Base bằng giao diện người dùng

1. Quay lại trình duyệt và mở trang web của chatbot.  
   Nếu bạn chưa đăng nhập, hãy sử dụng thông tin đăng nhập mà bạn đã lấy trước đó để đăng nhập.

2. Chọn tùy chọn **RAG with Knowledge Bases** từ menu.

3. Đặt một câu hỏi — bạn có thể sử dụng câu hỏi mẫu ở bảng bên phải. Dưới đây là một câu hỏi mẫu và phản hồi từ **Claude 3.5 Sonnet**.  Bạn có thể thử với các mô hình LLM khác và so sánh kết quả.


```text
What are Amazon sustainability goals by year 2040?
```

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/019.png?raw=true)

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/020.png?raw=true)
Nhiệm vụ này đã hướng dẫn bạn quy trình tạo và tích hợp Amazon Bedrock Knowledge Base với AWS Lambda và API Gateway.  
Bạn đã thiết lập thành công kiến trúc serverless sử dụng dữ liệu độc quyền của tổ chức để nâng cao các ứng dụng sử dụng trí tuệ nhân tạo.

Bằng cách kết nối Knowledge Base với Lambda và sử dụng API Gateway hiện có, bạn đã tạo ra một backend mạnh mẽ có khả năng xử lý truy vấn, truy xuất thông tin liên quan và tạo ra phản hồi theo ngữ cảnh.  

Việc tích hợp này mở ra nhiều khả năng để phát triển các ứng dụng thông minh — từ chatbot nâng cao đến giao diện tìm kiếm tinh vi — đồng thời vẫn đảm bảo **bảo mật dữ liệu** và **khả năng mở rộng**.
