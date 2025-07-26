---
title : "Tạo và Kiểm thử Knowledge Base"
date : 2025-07-17
weight : 9
chapter : false
pre : " <b> 9.1 </b> "
---

Việc tạo Amazon Bedrock Knowledge Base giúp tích hợp dữ liệu độc quyền vào ứng dụng AI bằng cách sử dụng kỹ thuật RAG (Retrieval Augmented Generation). Quá trình này chuyển đổi văn bản từ các nguồn dữ liệu thành các vector embedding bằng các mô hình như Amazon Titan Embeddings, sau đó lưu trữ trong cơ sở dữ liệu vector như OpenSearch Serverless. Khi hoàn tất, Knowledge Base có thể được truy vấn để lấy thông tin liên quan, từ đó bổ sung thêm ngữ cảnh cho các prompt gửi đến mô hình nền.

## Tạo Knowledge Base

1. Mở [Amazon Bedrock console](https://console.aws.amazon.com/bedrock).
2. Ở góc trên bên trái, chọn biểu tượng menu ☰. Trong thanh điều hướng, chọn **Knowledge Bases**.  
   ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/001.png?raw=true)
3. Chọn **Create knowledge base**, sau đó chọn **Knowledge base with vector store**  
   ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/002.png?raw=true)
4. Giữ nguyên tên mặc định.  
   ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/003.png?raw=true)
5. Ở phần **IAM permissions**, chọn **Use an existing role**.
6. Trong danh sách, chọn role `xxxx-BedrockExecutionRoleForKBs-xxxx`.  
   ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/004.png?raw=true)
7. Chọn **Amazon S3** làm nguồn dữ liệu.
8. Chọn **Next**.
9. Chọn **Browse S3**.
10. Chọn bucket `xxxx-s3bucket-xxxx` và thư mục **sample-documents**.  
    ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/007.png?raw=true)
11. Giữ nguyên cấu hình chia đoạn văn bản mặc định.  
12. Chọn **Next**.  
13. Trong phần **Embeddings model**, chọn **Titan Text Embeddings v2**.  
14. Trong phần **Vector database**, chọn **Quick create...**  
    ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/008.png?raw=true)  
    ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/009.png?raw=true)
15. Ở phần **Vector store**, chọn **Amazon OpenSearch Serverless**  
    ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/010.png?raw=true)
16. Chọn **Create knowledge base**.  
17. Quá trình này có thể mất vài phút để hoàn tất.  
    ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/011.png?raw=true)

## Đồng bộ nguồn dữ liệu với Knowledge Base

1. Trong phần **Data source**, chọn nguồn dữ liệu của bạn (*knowledge-base-quick-...*).  
2. Chọn nút **Sync**.  
   ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/014.png?raw=true)
3. Sau vài giây, Knowledge Base sẽ sẵn sàng để kiểm thử.

## Kiểm thử Knowledge Base

1. Chọn Knowledge Base của bạn, sau đó chọn nút **Test knowledge base**.  
   ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/015.png?raw=true)
2. Ở khung bên phải, chọn **Select model**.
3. Trong cửa sổ **Select model**, chọn **Claude 3 Haiku**, rồi chọn **Apply**.  
   _(Lưu ý: Bạn cũng có thể thử nghiệm với các mô hình khác nếu chúng đã được kích hoạt.)_  
   ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/016.png?raw=true)
4. Nhập câu hỏi sau vào ô _Enter your message here_:

    ```text
    What are Amazon sustainability goals by year 2040?
    ```

5. Chọn **Run**.
6. Bạn sẽ thấy phản hồi như hình dưới.  
   ![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/8.KB/017.png?raw=true)

Bạn hãy khám phá khả năng của Knowledge Base bằng cách đặt nhiều câu hỏi khác nhau. Một tính năng đáng chú ý là khả năng ghi nhớ ngữ cảnh trong suốt cuộc hội thoại. Điều này cho phép bạn hỏi tiếp nối từ câu trả lời trước đó, giúp truy vấn sâu hơn và khai thác thông tin chi tiết hơn từ dữ liệu của bạn.
