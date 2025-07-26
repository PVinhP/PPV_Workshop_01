---
title : "Khởi chạy CloudFormation stack"
date : 2025-06-17
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

{{% notice info %}}
**Regions được hỗ trợ**: Bạn nên chạy Workshop này ở Region **us-west-2**.
{{% /notice %}} 

Một template AWS CloudFormation được sử dụng để thiết lập các tài nguyên lab trong AWS Region mà bạn chọn. Bước này là bắt buộc vì các hướng dẫn sau đây dựa trên những tài nguyên này. Template CloudFormation tạo ra các tài nguyên AWS sau:

- **VSCode**: VSCode trên Amazon EC2 là một môi trường phát triển tích hợp (IDE) dựa trên cloud mà bạn có thể sử dụng để viết, chạy và debug code chỉ với một trình duyệt. Nó bao gồm trình soạn thảo code, debugger và terminal. Trong workshop này, bạn sử dụng VSCode editor để triển khai một ứng dụng backend, được xây dựng bằng AWS Serverless Application Model (AWS SAM), và cũng triển khai AWS Amplify frontend.
- **Amazon S3**: Amazon Simple Storage Service (Amazon S3) là một dịch vụ lưu trữ đối tượng cung cấp khả năng mở rộng hàng đầu trong ngành, tính khả dụng của dữ liệu, bảo mật và hiệu suất. Khách hàng ở mọi quy mô và ngành công nghiệp có thể lưu trữ và bảo vệ bất kỳ lượng dữ liệu nào cho hầu như mọi trường hợp sử dụng, chẳng hạn như data lake, ứng dụng tập trung cloud và ứng dụng di động. Trong workshop này, bạn sử dụng một S3 bucket để tải lên các tài liệu trung tâm trường hợp sử dụng của mình. Amazon Kendra lập chỉ mục các tài liệu này để cung cấp câu trả lời Retrieval-Augmented Generation (RAG) cho các câu hỏi mà bạn đặt ra.
- **Amazon Kendra**: Amazon Kendra là một dịch vụ tìm kiếm doanh nghiệp thông minh giúp bạn tìm kiếm qua các kho lưu trữ nội dung khác nhau với các connector tích hợp sẵn.

Tải xuống template CloudFormation:

1. Tải xuống template CloudFormation: [<span style="background-color:#f90; color:#000; padding:6px 14px; border-radius:10px;">Tải xuống</span>](https://static.us-east-1.prod.workshops.aws/public/c5c516a7-10ce-444b-a0c5-1e60794fdb7c/static/template/chatbot-startup-stack.yaml)
2. Lưu trữ file YAML template trong một thư mục trên máy local của bạn.
3. Điều hướng đến [**AWS CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home) 🔗
4. Trên CloudFormation console, chọn **Upload a template file**.
5. Chọn template mà bạn vừa tải xuống, sau đó chọn [<span style="background-color:#1b64da; color:#000; padding:6px 14px; border-radius:10px;">Next</span>](#)

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh_1.png?raw=true)

6. Đặt tên cho stack, chẳng hạn như `chatbot-startup-stack`

7. Giữ các giá trị khác không thay đổi, và chọn **`Next`**

8. Đối với **Configure stack options**, chọn các tùy chọn *I acknowledge...* và chọn **`Next`**

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh_2.png?raw=true)
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh_3.png?raw=true)

9. Để triển khai template, chọn **`Submit`**
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh_4.png?raw=true)

10. Sau khi template được triển khai, để xem lại các tài nguyên đã tạo, điều hướng đến [CloudFormation Resources](#), và sau đó chọn CloudFormation stack mà bạn đã tạo.

> ⏳ *Việc triển khai template mất 10–15 phút để hoàn thành tất cả việc cung cấp tài nguyên AWS.*

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh5.png?raw=true)

**Chúc mừng! Bây giờ bạn có thể tiến hành nhiệm vụ tiếp theo.**

---