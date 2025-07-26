---
title : "Trò chuyện RAG với Bedrock Knowledge Bases"
date : 2025-07-17
weight : 9
chapter : false
pre : " <b> 9. </b> "
---

Amazon Bedrock Knowledge Bases là một tính năng được quản lý toàn phần, cho phép bạn triển khai quy trình RAG (Retrieval Augmented Generation) dựa trên nguồn dữ liệu riêng của tổ chức. Bedrock Knowledge Bases tự động hóa toàn bộ quy trình RAG — từ việc nạp dữ liệu, truy xuất thông tin, đến bổ sung vào prompt — mà không cần tích hợp thủ công hoặc tự quản lý luồng dữ liệu. Điều này giúp bạn cung cấp cho các mô hình nền tảng (Foundation Models - FMs) và tác nhân (Agents) thông tin cập nhật và riêng biệt, từ đó tạo ra phản hồi chính xác và phù hợp hơn.

Amazon Bedrock Knowledge Bases dựa vào hai thành phần chính để thực hiện việc truy xuất thông tin hiệu quả: **embeddings** và **vector stores**. Hai thành phần này hoạt động cùng nhau để chuyển đổi dữ liệu văn bản thành định dạng có thể tìm kiếm nhanh chóng dựa trên sự tương đồng về ngữ nghĩa.

**Embeddings** là biểu diễn số của văn bản nhằm nắm bắt ý nghĩa ngữ nghĩa. Trong Amazon Bedrock, các mô hình embedding như **Amazon Titan Embeddings** hoặc **Cohere Embed** sẽ chuyển văn bản từ tài liệu của bạn thành các vector đặc (dense vectors). Quá trình này cho phép hệ thống so sánh và truy xuất nội dung có ý nghĩa tương tự một cách hiệu quả, tạo nền tảng cho khả năng “hiểu” dữ liệu của Knowledge Base.

**Vector stores** là cơ sở dữ liệu chuyên biệt được thiết kế để lập chỉ mục và truy vấn các embedding vector một cách hiệu quả. Amazon Bedrock hỗ trợ các tuỳ chọn như **Amazon OpenSearch Serverless được quản lý bởi Amazon** hoặc giải pháp tùy chỉnh như **Amazon Aurora PostgreSQL với tiện ích mở rộng pgvector**. Các vector store này cho phép thực hiện truy vấn theo ngữ nghĩa nhanh chóng, giúp hệ thống nhận diện và truy xuất thông tin phù hợp nhất khi xử lý truy vấn — từ đó nâng cao hiệu quả của quy trình RAG.

## Chức năng và Lợi ích

**Triển khai RAG liền mạch**: Amazon Bedrock Knowledge Bases tự động hóa toàn bộ quy trình RAG — từ nạp dữ liệu, truy xuất thông tin, đến bổ sung vào prompt — mà không cần tích hợp thủ công hoặc tự quản lý luồng dữ liệu. Điều này cho phép bạn cung cấp cho mô hình nền tảng (FMs) và các tác nhân (Agents) thông tin cập nhật và độc quyền để tạo ra phản hồi chính xác hơn.

**Kết nối dữ liệu an toàn**: Dịch vụ sẽ tự động truy xuất tài liệu từ các nguồn dữ liệu mà bạn chỉ định, bao gồm Amazon S3, Web Crawler, Salesforce, và SharePoint. Sau đó, nội dung sẽ được xử lý, chia nhỏ thành các khối văn bản, chuyển thành embeddings, và lưu trữ trong vector database.

**Tuỳ chỉnh linh hoạt**: Bạn có thể tinh chỉnh cả quá trình nạp dữ liệu và truy xuất để cải thiện độ chính xác cho từng trường hợp sử dụng cụ thể. Các tùy chọn phân tích nâng cao giúp hiểu dữ liệu phi cấu trúc phức tạp, và bạn có thể chọn nhiều chiến lược chia nhỏ nội dung (chunking) hoặc tự viết mã chia nhỏ theo nhu cầu.

## Kiến trúc của Knowledge Bases

Trong hai tác vụ tiếp theo, bạn sẽ xây dựng một chatbot sử dụng Amazon Bedrock Knowledge Bases. Kiến trúc tổng thể của giải pháp được minh hoạ bên dưới:  
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/000-architecture.png?raw=true)

## Các thành phần của Knowledge Base và Vector Search

1. **Knowledge Bases**: Kho lưu trữ trung tâm chứa thông tin có cấu trúc  
2. **Amazon S3**: Nơi lưu trữ tài liệu (pdf, csv, txt, v.v.)  
3. **Amazon OpenSearch**: Vector database và công cụ tìm kiếm cho truy vấn tương đồng hiệu quả  
4. **Amazon Bedrock**: Cung cấp các mô hình embedding như Amazon Titan Embeddings

---

## Quy trình làm việc của Knowledge Base và Vector Search

1. Tài liệu được lưu trữ trong Amazon S3  
2. Văn bản được trích xuất và xử lý từ các tài liệu  
3. Mô hình Titan Embeddings của Amazon Bedrock tạo ra vector đại diện cho văn bản  
4. Các vector được lưu vào Amazon OpenSearch và đóng vai trò là vector database  
5. Knowledge Bases thu nhận và cấu trúc thông tin, tích hợp các vector đã tạo  
6. Các hàm AWS Lambda (RAG/KB/LLM Functions) tương tác với Knowledge Bases và vector search  
7. RAG (Retrieval Augmented Generation) sử dụng tìm kiếm tương đồng vector để truy xuất thông tin liên quan

## 🔍 Các tính năng chính của tích hợp Knowledge Base và Vector Search

- Tìm kiếm theo ngữ nghĩa sử dụng vector biểu diễn văn bản  
- Tìm kiếm tương đồng hiệu quả thông qua chức năng vector database của Amazon OpenSearch  
- Tích hợp Amazon Titan Embeddings để vector hóa văn bản chất lượng cao  
- Cải thiện ngữ cảnh và độ chính xác trong phản hồi chatbot thông qua truy xuất dựa trên vector  
- Nâng cao mức độ liên quan trong quá trình truy xuất thông tin phục vụ cho RAG  
- Có khả năng xử lý và tìm kiếm khối lượng lớn dữ liệu văn bản phi cấu trúc  
- Kết hợp mượt mà giữa tìm kiếm theo từ khóa truyền thống và tìm kiếm theo ngữ nghĩa dựa trên vector

---

Thiết lập này cho phép chatbot thực hiện các truy vấn ngữ nghĩa nâng cao trên nền tảng kiến thức doanh nghiệp.  
Bằng cách sử dụng Amazon Titan Embeddings để tạo vector đại diện cho văn bản và lưu trữ chúng trong Amazon OpenSearch như một vector database, hệ thống có thể tìm ra thông tin liên quan về mặt ngữ cảnh ngay cả khi không có từ khóa khớp chính xác.  
Điều này giúp chatbot hiểu và phản hồi truy vấn người dùng với thông tin phù hợp hơn từ kho tri thức của doanh nghiệp.
