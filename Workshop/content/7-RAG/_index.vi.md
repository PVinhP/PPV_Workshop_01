---
title : "Trò chuyện RAG với nhiều mô hình LLM"
date : 2025-07-17
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

Bạn đã triển khai thành công ứng dụng chatbot serverless sử dụng Amazon Bedrock và đã cấp quyền truy cập cho các Mô hình Ngôn ngữ Lớn (LLMs) cần thiết thông qua bảng điều khiển Amazon Bedrock. Ngoài ra, bạn cũng đã hoàn tất thiết lập Amazon Kendra.

## Giải pháp dựa trên RAG với Amazon Bedrock và LangChain

Giải pháp trong workshop này được xây dựng dựa trên phương pháp Retrieval-Augmented Generation (RAG). RAG là một kiến trúc mô hình kết hợp giữa kỹ thuật truy xuất và sinh văn bản nhằm nâng cao chất lượng và mức độ phù hợp của nội dung được tạo ra. Khi bạn nhập một câu hỏi vào ô văn bản, các bước xử lý sau sẽ được thực hiện phía backend để cung cấp cho bạn câu trả lời dựa trên tài liệu nguồn:

**Retrieval (Truy xuất)**: Quá trình này tìm kiếm trong một tập hợp văn bản lớn để xác định thông tin hoặc ngữ cảnh phù hợp. Trong bước này, Amazon Kendra nhận câu hỏi từ yêu cầu và tìm kiếm các câu trả lời cũng như tài liệu tham khảo liên quan.

**Augmentation (Tăng cường)**: Sau khi truy xuất được thông tin phù hợp, mô hình sử dụng ngữ cảnh đó để tăng cường khả năng sinh văn bản. Điều này giúp nội dung tạo ra có tính liên quan về ngữ cảnh và giàu thông tin.

**Generation (Sinh nội dung)**: Trong RAG, đây là quá trình mô hình tạo ra văn bản mới dựa trên ngữ cảnh đã truy xuất và tăng cường. Văn bản được sinh ra có thể là câu trả lời, phản hồi hoặc giải thích.

**LangChain**: Để điều phối toàn bộ quy trình này, workshop sử dụng tác nhân LangChain. Các lớp trừu tượng linh hoạt và bộ công cụ toàn diện của LangChain giúp nhà phát triển tận dụng tối đa sức mạnh của các mô hình nền tảng (Foundation Models - FMs).

Trong tác vụ này, bạn sẽ triển khai một hàm Lambda RAG (Retrieve, Analyze, Generate) để cung cấp trải nghiệm chatbot theo ngữ cảnh với tập dữ liệu của bạn. Các tập dữ liệu mẫu được lưu trữ trên Amazon S3 và được lập chỉ mục bằng Amazon Kendra. Để điều phối luồng giữa truy vấn người dùng, chỉ mục Kendra và các mô hình LLMs, bạn sẽ sử dụng LangChain làm công cụ điều phối. Mã Lambda được cung cấp sử dụng API của LangChain để trừu tượng hóa logic tích hợp phức tạp này.

Bằng cách tận dụng sức mạnh của Amazon Bedrock, Amazon Kendra và LangChain, bạn có thể xây dựng một trải nghiệm chatbot liền mạch và theo ngữ cảnh cho người dùng, giúp họ tương tác với tập dữ liệu một cách tự nhiên và hiệu quả.

Trong phần này, bạn sẽ cập nhật và triển khai một hàm Lambda RAG. Hàm này cho phép trò chuyện theo ngữ cảnh với nhiều mô hình ngôn ngữ lớn khác nhau.

1. Mở trình soạn thảo **VSCode**.

2. Từ dự án **bedrock-serverless-workshop**, mở hàm **/lambdas/ragFunctions/ragfunction.py**, sao chép đoạn mã bên dưới và cập nhật mã trong hàm. Hàm này chứa logic hỗ trợ các mô hình Claude 3 (Haiku, Sonnet, v.v.), Mistral và Llama.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/007.png?raw=true)

````bash
import os
import json
import boto3
from langchain_community.retrievers import AmazonKendraRetriever
from langchain_aws import ChatBedrock
from langchain.chains import ConversationalRetrievalChain
from langchain.memory import ConversationBufferMemory
from langchain.prompts import PromptTemplate
from langchain.chains import RetrievalQA

import traceback

kendra = boto3.client('kendra')
chain_type = 'stuff'

KENDRA_INDEX_ID = os.getenv('KENDRA_INDEX_ID')
S3_BUCKET_NAME = os.environ["S3_BUCKET_NAME"]


refine_prompt_template = (
    "Below is an instruction that describes a task. "
    "Write a response that appropriately completes the request.\n\n"
    "### Instruction:\n"
    "This is the original question: {question}\n"
    "The existing answer: {existing_answer}\n"
    "Now there are some additional texts, (if needed) you can use them to improve your existing answer."
    "\n\n"
    "{context_str}\n"
    "\\nn"
    "Please use the new passage to further improve your answer.\n\n"
    "### Response: "
)

initial_qa_template = (
    "Below is an instruction that describes a task. "
    "Write a response that appropriately completes the request.\n\n"
    "### Instruction:\n"
    "The following is background knowledge：\n"
    "{context_str}"
    "\n"
    "Please answer this question based on the background knowledge provided above：{question}。\n\n"
    "### Response: "
)

def lambda_handler(event, context):
    print(f"Event is: {event}")
    
    event_body = json.loads(event["body"])
    question = event_body["query"]
    print(f"Query is: {question}")
    
    model_id = event_body["model_id"]
    temperature = event_body["temperature"]
    max_tokens = event_body["max_tokens"]

    response = ''
    status_code = 200

    PROMPT_TEMPLATE = 'prompt-engineering/claude-prompt-template.txt'

    try:
        if model_id == 'mistral.mistral-7b-instruct-v0:2':
            llm = get_mistral_llm(model_id,temperature,max_tokens)
            PROMPT_TEMPLATE = 'prompt-engineering/mistral-prompt-template.txt'
        elif model_id == 'meta.llama3-1-8b-instruct-v1:0':
            llm = get_llama_llm(model_id,temperature,max_tokens)
            PROMPT_TEMPLATE = 'prompt-engineering/llama-prompt-template.txt'
        else:
            llm = get_claude_llm(model_id,temperature,max_tokens)
            PROMPT_TEMPLATE = 'prompt-engineering/claude-prompt-template.txt'
        
        # Read the prompt template from S3 bucket
        s3 = boto3.resource('s3')
        obj = s3.Object(S3_BUCKET_NAME, PROMPT_TEMPLATE) 
        prompt_template = obj.get()['Body'].read().decode('utf-8')
        print(f"prompt template: {prompt_template}")
        
        retriever = AmazonKendraRetriever(kendra_client=kendra,index_id=KENDRA_INDEX_ID)
        
        
        if chain_type == "stuff":
            PROMPT = PromptTemplate(
                template=prompt_template, input_variables=["context", "question"]
            )
            chain_type_kwargs = {"prompt": PROMPT}
            qa = RetrievalQA.from_chain_type(
                llm=llm,
                chain_type="stuff",
                retriever=retriever,
                return_source_documents=True,
                chain_type_kwargs=chain_type_kwargs)
            response = qa(question, return_only_outputs=False)
        elif chain_type == "refine":
            refine_prompt = PromptTemplate(
                input_variables=["question", "existing_answer", "context_str"],
                template=refine_prompt_template,
            )
            initial_qa_prompt = PromptTemplate(
                input_variables=["context_str", "question"],
                template=prompt_template,
            )
            chain_type_kwargs = {"question_prompt": initial_qa_prompt, "refine_prompt": refine_prompt}
            qa = RetrievalQA.from_chain_type(
                llm=llm, 
                chain_type="refine",
                retriever=retriever,
                return_source_documents=True,
                chain_type_kwargs=chain_type_kwargs)
            response = qa(question, return_only_outputs=False)
                
        print('Response', response)
        source_documents = response.get('source_documents')
        source_docs = []
        previous_source = None
        previous_score = None
        response_data = []
        
        #if chain_type == "stuff":
        for source_doc in source_documents:
            source = source_doc.metadata['source']
            score = source_doc.metadata["score"]
            if source != previous_source or score != previous_score:
                source_data = {
                    "source": source,
                    "score": score
                }
                response_data.append(source_data)
                previous_source = source
                previous_score = score
        
        response_with_metadata = {
            "answer": response.get('result'),
            "source_documents": response_data
        }

        return {
            'statusCode': status_code,
            'headers': {
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Headers': 'Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token',
                'Access-Control-Allow-Methods': 'OPTIONS,POST'
            },
            'body': json.dumps(response_with_metadata)
        }
    
        
    except Exception as e:
        print(f"An unexpected error occurred: {str(e)}")
        stack_trace = traceback.format_exc()
        print(f"stack trace: {stack_trace}")
        print(f"error: {str(e)}")
        
        response = str(e)
        return {
            'statusCode': status_code,
            'headers': {
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Headers': 'Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token',
                'Access-Control-Allow-Methods': 'OPTIONS,POST'
            },
            'body': json.dumps({'error': response})
        }
        
def get_claude_llm(model_id, temperature, max_tokens):
    model_kwargs = {
        "max_tokens": max_tokens,
        "temperature": temperature, 
        "top_k": 50, 
        "top_p": 0.95
    }
    llm = ChatBedrock(model_id=model_id, model_kwargs=model_kwargs) 
    return llm

def get_llama_llm(model_id, temperature, max_tokens):
    model_kwargs = {
        "max_gen_len": max_tokens,
        "temperature": temperature, 
        "top_p": 0.9
    }
    llm = ChatBedrock(model_id=model_id, model_kwargs=model_kwargs) 
    return llm

def get_mistral_llm(model_id, temperature, max_tokens):
    model_kwargs = { 
        "max_tokens": max_tokens,
        "temperature": temperature, 
        "top_k": 50, 
        "top_p": 0.9
    }
    llm = ChatBedrock(model_id=model_id, model_kwargs=model_kwargs) 
    return llm

def get_memory():

    memory = ConversationBufferMemory(
        memory_key="chat_history",
        input_key="question",
        output_key="answer",
        return_messages=True
    )
    return memory

````
3. Từ terminal của **VSCode**, chạy lệnh sau để build và triển khai với mã Lambda đã được cập nhật:

````bash
cd ~/environment/bedrock-serverless-workshop
sam build && sam deploy

````
Trong tác vụ này, bạn sẽ có cơ hội làm việc với các mô hình LLM sau, mỗi mô hình đều mang đến các tính năng và thế mạnh riêng biệt:

- **anthropic.claude-3-haiku - Claude 3 Haiku** là mô hình nhanh nhất và nhỏ gọn nhất của Anthropic, mang lại khả năng phản hồi gần như tức thì. Haiku là lựa chọn tối ưu để xây dựng trải nghiệm AI liền mạch, mô phỏng tương tác tự nhiên như con người.
- **anthropic.claude-3-5-sonnet - Claude 3.5 Sonnet** là mô hình tiên tiến và thông minh nhất của Anthropic, thể hiện năng lực vượt trội trong nhiều tác vụ và bài đánh giá khác nhau.
- **anthropic.claude-3-opus - Claude 3 Opus** là một mô hình cực kỳ thông minh với hiệu năng ổn định trên các tác vụ phức tạp. Opus có thể xử lý các yêu cầu mở, chưa từng thấy trước đó với độ trôi chảy cao và khả năng hiểu giống con người. Hãy sử dụng Opus để tự động hóa công việc, tăng tốc nghiên cứu và phát triển trong nhiều lĩnh vực và ngành nghề khác nhau.
- **mistral.mistral-7b-instruct - Mistral** là một mô hình ngôn ngữ lớn hiệu quả cao, được tối ưu hóa cho các tác vụ ngôn ngữ yêu cầu dung lượng lớn và độ trễ thấp. Các trường hợp sử dụng phổ biến của Mistral bao gồm: tóm tắt văn bản, cấu trúc lại thông tin, trả lời câu hỏi và hoàn thành mã nguồn.
- **meta.llama3-1-8b-instruct - Llama 3.1 8B** phù hợp nhất với các hệ thống có tài nguyên tính toán giới hạn. Mô hình này đặc biệt mạnh trong các tác vụ như tóm tắt văn bản, phân loại văn bản, phân tích cảm xúc và dịch ngôn ngữ với yêu cầu suy luận có độ trễ thấp.

4. Quay lại trình duyệt và mở trang web chatbot. Nhấp vào liên kết **RAG Chat with LLMs**. Giao diện trang web sẽ hiển thị như sau:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/008.png?raw=true)

5. Đặt bất kỳ câu hỏi nào bằng cách nhập vào ô Query và nhấn nút **Ask Question**. Bạn có thể thử một câu hỏi mẫu ở bảng bên phải. Dưới đây là một ví dụ về câu hỏi và phản hồi từ Claude 3 Haiku. Bạn cũng có thể thử với các mô hình LLM khác và so sánh kết quả.

````bash
What is federal funds rate as of September 2024?
````

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/009.png?raw=true)
