---
title : "Giới thiệu"
date :  2025-06-17
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
# Tạo Chatbot Serverless Sử Dụng Amazon Bedrock, Amazon Kendra và Dữ Liệu Của Riêng Bạn
### Tổng quan
Trong workshop này, bạn sẽ bắt đầu một hành trình toàn diện để tạo ra một chatbot serverless sử dụng các công nghệ AI tổng hợp tiên tiến. Bạn sẽ tận dụng Amazon Bedrock, Amazon Kendra và dữ liệu của riêng mình để xây dựng một giao diện đối thoại thông minh có thể truy xuất và tạo ra các phản hồi phù hợp theo ngữ cảnh. Workshop sẽ hướng dẫn bạn cấu hình nhiều dịch vụ bao gồm AWS Lambda, API Gateway và AWS Amplify, đồng thời bạn sẽ làm việc với các mô hình ngôn ngữ lớn khác nhau như Claude 3, Mistral và Llama. Khi kết thúc workshop, bạn sẽ có một chatbot hoàn chỉnh có thể tương tác với cơ sở tri thức doanh nghiệp sử dụng các kỹ thuật Retrieval-Augmented Generation (RAG). Workshop này giải quyết các trường hợp sử dụng thực tế nơi các tổ chức cần giao diện đối thoại thông minh, dựa trên dữ liệu.

Bạn sẽ học cách giải quyết các thách thức như khám phá tri thức doanh nghiệp, tự động hóa hỗ trợ khách hàng và truy xuất thông tin qua các kho tài liệu phức tạp. Giải pháp bạn sẽ xây dựng có thể được áp dụng trong nhiều tình huống kinh doanh, bao gồm hệ thống hỗ trợ kỹ thuật, quản lý tri thức nhân sự, truy vấn tài liệu tuân thủ và tổng hợp thông tin nghiên cứu. Bằng cách thành thạo các kỹ thuật này, bạn sẽ được trang bị để tạo ra các giải pháp được hỗ trợ bởi AI có thể biến đổi cách thức các doanh nghiệp tương tác với dữ liệu của họ, cung cấp các phản hồi nhanh hơn, chính xác hơn và nhận thức ngữ cảnh đối với các truy vấn phức tạp.

Trong suốt workshop này, bạn sẽ có được kiến thức và kỹ năng sau:

- Cách xây dựng các giải pháp AI tổng hợp bằng cách sử dụng Amazon Bedrock và kiến trúc serverless.
- Cách sử dụng AWS SAM để tạo và triển khai các giải pháp AI tổng hợp.
- Cách khai thác khả năng của các Mô hình Ngôn ngữ Lớn thông qua kiến trúc Retrieval-Augmented Generation (RAG).
- Tầm quan trọng của quản lý dữ liệu hiệu quả trong bối cảnh AI tổng hợp, bao gồm việc tìm nguồn, lưu trữ và tiền xử lý dữ liệu để nâng cao hiệu suất và tính liên quan của các phản hồi được tạo bởi AI.
- Vai trò của Kỹ thuật Prompt Engineering trong việc đạt được kết quả tốt hơn.

Bạn sẽ tạo ra kiến trúc sau đây cho workshop này:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/000-architecture.png?raw=true)
### 🧩 Các Thành phần

- **Users**: Người dùng cuối của chatbot  
- **Web UI**: Giao diện người dùng cho việc tương tác với chatbot  
- **AWS Amplify (React, Vue.js)**: Quản lý front-end và xác thực, đặc biệt sử dụng React hoặc Vue.js  
- **Amazon API Gateway**: Xử lý các yêu cầu API  
- **AWS Lambda (RAG/KB/LLM Functions)**: Thực thi các hàm serverless cho Retrieval-Augmented Generation, các thao tác Knowledge Base và tương tác LLM  
- **Amazon Bedrock**: Cung cấp quyền truy cập vào các mô hình và dịch vụ AI  
- **Large Language Models (Claude 3, Mistral, Llama etc.)**: Các mô hình AI hỗ trợ phản hồi  
- **Knowledge Bases**: Lưu trữ thông tin có cấu trúc  
- **Amazon S3**: Lưu trữ đối tượng cho tài liệu và dữ liệu  
- **Documents (PDF, CSV, TXT etc.)**: Các loại tệp khác nhau để thu thập  
- **Amazon Kendra**: Dịch vụ tìm kiếm thông minh  
- **Amazon OpenSearch**: Cơ sở dữ liệu vector và công cụ tìm kiếm cho tìm kiếm tương đồng hiệu quả  
- **Amazon Cognito**: Xác thực và ủy quyền người dùng  

---

### 🔄 Quy trình Làm việc

1. Người dùng tương tác với Web UI  
2. Các yêu cầu được định tuyến qua API Gateway đến các hàm Lambda  
3. Các hàm Lambda sử dụng Bedrock để truy cập LLM, thao tác RAG và tương tác Knowledge Base  
4. Tri thức được truy xuất từ Knowledge Bases, S3, Kendra hoặc OpenSearch  
5. Hệ thống tích hợp các loại tài liệu khác nhau để nâng cao cơ sở tri thức  

---

### ✨ Tính năng Chính

- Kiến trúc serverless có thể mở rộng  
- Tận dụng các mô hình LLM khác nhau bao gồm Claude 3, Mistral và Llama  
- Tích hợp tri thức doanh nghiệp thông qua Kendra và các cơ sở tri thức tùy chỉnh  
- Xác thực bảo mật với Cognito  
- Khả năng thu thập tài liệu và tìm kiếm linh hoạt  
- Front-end được xây dựng với các framework hiện đại (React hoặc Vue.js) sử dụng AWS Amplify  

---

Kiến trúc này cho phép tạo ra một **giải pháp chatbot được hỗ trợ bởi AI tổng hợp** có thể mở rộng theo nhu cầu trong khi cung cấp **các phản hồi thông minh** dựa trên cả **tri thức của LLM và tri thức đặc thù doanh nghiệp**. Việc sử dụng **React hoặc Vue.js với AWS Amplify** đảm bảo một giao diện người dùng phản hồi nhanh và hiệu quả.


### Amazon Kendra

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/Kendra.png?raw=true)

**Amazon Kendra** là một dịch vụ truy xuất thông tin được quản lý và tìm kiếm thông minh sử dụng xử lý ngôn ngữ tự nhiên và mô hình deep learning tiên tiến. Khác với tìm kiếm dựa trên từ khóa truyền thống, Amazon Kendra sử dụng độ tương đồng ngữ nghĩa và ngữ cảnh—cùng với khả năng xếp hạng—để quyết định xem một đoạn văn bản hoặc tài liệu có liên quan đến truy vấn truy xuất hay không.

Nguồn: [Amazon Kendra – What is Kendra?](https://docs.aws.amazon.com/kendra/latest/dg/what-is-kendra.html)  
 *Nguồn này cung cấp giải thích chi tiết hơn về cách Amazon Kendra hoạt động.*

### Amazon Bedrock

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/Bedrock.png?raw=true)
**Amazon Bedrock** là một dịch vụ được quản lý hoàn toàn giúp các mô hình nền tảng (FMs) hiệu suất cao từ các công ty AI hàng đầu và Amazon có sẵn để bạn sử dụng thông qua một API thống nhất. Bạn có thể chọn từ một loạt các mô hình nền tảng để tìm mô hình phù hợp nhất cho trường hợp sử dụng của mình. Amazon Bedrock cũng cung cấp một bộ khả năng rộng để xây dựng các ứng dụng AI tổng hợp với tính bảo mật, quyền riêng tư và AI có trách nhiệm. Sử dụng Amazon Bedrock, bạn có thể dễ dàng thử nghiệm và đánh giá các mô hình nền tảng hàng đầu cho các trường hợp sử dụng của mình, tùy chỉnh riêng tư chúng với dữ liệu của bạn sử dụng các kỹ thuật như fine-tuning và Retrieval Augmented Generation (RAG), và xây dựng các agent thực hiện các nhiệm vụ sử dụng các hệ thống doanh nghiệp và nguồn dữ liệu của bạn.

Với trải nghiệm serverless của Amazon Bedrock, bạn có thể bắt đầu nhanh chóng, tùy chỉnh riêng tư các mô hình nền tảng với dữ liệu của riêng mình, và dễ dàng cũng như bảo mật tích hợp và triển khai chúng vào các ứng dụng của bạn sử dụng các công cụ AWS mà không cần quản lý bất kỳ cơ sở hạ tầng nào.

Nguồn: [Amazon Bedrock – What is Amazon Bedrock?](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html)  
 *Nguồn này cung cấp giải thích chi tiết hơn về cách Amazon Bedrock hoạt động.*