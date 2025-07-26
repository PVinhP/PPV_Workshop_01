# Tạo Chatbot Serverless Sử Dụng Amazon Bedrock, Amazon Kendra và Dữ Liệu Của Riêng Bạn

### Tổng quan
Trong workshop này, chúng ta sẽ bắt đầu một hành trình toàn diện để tạo ra một chatbot serverless sử dụng các công nghệ AI tổng hợp tiên tiến. chúng ta sẽ tận dụng Amazon Bedrock, Amazon Kendra và dữ liệu của riêng mình để xây dựng một giao diện đối thoại thông minh có thể truy xuất và tạo ra các phản hồi phù hợp theo ngữ cảnh. Workshop sẽ hướng dẫn bạn cấu hình nhiều dịch vụ bao gồm AWS Lambda, API Gateway và AWS Amplify, đồng thời bạn sẽ làm việc với các mô hình ngôn ngữ lớn khác nhau như Claude 3, Mistral và Llama. Khi kết thúc workshop, chúng ta sẽ có một chatbot hoàn chỉnh có thể tương tác với cơ sở tri thức doanh nghiệp sử dụng các kỹ thuật Retrieval-Augmented Generation (RAG). Workshop này giải quyết các trường hợp sử dụng thực tế nơi các tổ chức cần giao diện đối thoại thông minh, dựa trên dữ liệu.

Bạn sẽ được học cách giải quyết các thách thức như khám phá tri thức doanh nghiệp, tự động hóa hỗ trợ khách hàng và truy xuất thông tin qua các kho tài liệu phức tạp. Giải pháp bạn sẽ xây dựng có thể được áp dụng trong nhiều tình huống kinh doanh, bao gồm hệ thống hỗ trợ kỹ thuật, quản lý tri thức nhân sự, truy vấn tài liệu tuân thủ và tổng hợp thông tin nghiên cứu. Bằng cách thành thạo các kỹ thuật này, bạn sẽ được trang bị để tạo ra các giải pháp được hỗ trợ bởi AI có thể biến đổi cách thức các doanh nghiệp tương tác với dữ liệu của họ, cung cấp các phản hồi nhanh hơn, chính xác hơn và nhận thức ngữ cảnh đối với các truy vấn phức tạp.

Bạn sẽ tạo ra kiến trúc sau đây cho workshop này:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/000-architecture.png?raw=true)



### Nội dung
1. [Giới thiệu](1-Introduce/)
2. [Chuẩn bị](2-Prerequiste/)
3. [Triển khai Ứng dụng Serverless Amazon Bedrock](3-Accessibilitytoinstances/)
4. [Hướng dẫn Amazon Bedrock Console](4-Walkthroughbedrock/)
5. [Chat Tổng quát với Nhiều LLM](5-MultipleLLMs/)
6. [Lập chỉ mục Dữ liệu Nguồn bằng Amazon Kendra](6-UsingAmazonKendra/)
7. [RAG Chat với Nhiều LLM](7-RAG/)
8. [Kỹ thuật Prompt Engineering](8-PromptEngineering/)
9. [RAG Chat với Bedrock Knowledge Bases](9-KnowledgeBases/)
10. [Dọn dẹp Tài nguyên](10-cleanup/)

# Nguồn tham khảo: 
https://catalog.us-east-1.prod.workshops.aws/workshops/27eb3134-4f33-4689-bb73-269e4273947a/en-US 
