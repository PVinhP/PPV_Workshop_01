---
title : "Lập chỉ mục dữ liệu nguồn bằng Amazon Kendra"
date : 2025-07-17
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

Trong kiến trúc RAG, Amazon Kendra có thể được sử dụng để lập chỉ mục và tìm kiếm trên tập hợp tài liệu mẫu được lưu trữ trong Amazon S3 hoặc các nguồn khác. Người dùng có thể đưa ra một truy vấn hoặc câu hỏi, và Kendra sẽ thực hiện tìm kiếm theo độ tương đồng trên nội dung đã được lập chỉ mục để xác định thông tin liên quan nhất.

Khả năng xử lý ngôn ngữ tự nhiên nâng cao của Kendra cho phép nó hiểu ý định và truy vấn của người dùng, sau đó truy xuất nội dung phù hợp nhất từ các nguồn dữ liệu đã được lập chỉ mục. Điều này giúp người dùng nhanh chóng tìm được thông tin cần thiết mà không cần phải xem qua từng tài liệu một cách thủ công.

Bằng cách tích hợp Kendra vào giai đoạn "Retrieve" trong kiến trúc RAG, các tổ chức có thể nâng cao khả năng tìm kiếm và khai phá thông tin, từ đó hỗ trợ tốt hơn cho việc phân tích và tạo ra các phản hồi có giá trị. Việc tích hợp liền mạch giữa Kendra và Amazon S3 giúp đơn giản hóa quá trình lập chỉ mục và quản lý nội dung, khiến Kendra trở thành một công cụ mạnh mẽ trong hệ thống RAG.

## Tải xuống tài liệu mẫu

Tải về một vài tài liệu mẫu để kiểm thử giải pháp này. Tài liệu đầu tiên là biên bản cuộc họp tháng 5 và tháng 9 năm 2024 của Ủy ban Thị trường Mở Liên bang (FOMC). Tài liệu thứ hai là Báo cáo Phát triển Bền vững năm 2023 của Amazon. Tài liệu thứ ba là báo cáo 10-K năm 2023 của Henry Schein, một nhà cung cấp dịch vụ nha khoa. Bạn có thể tải về hoặc sử dụng bất kỳ tài liệu nào để thử nghiệm, hoặc dùng dữ liệu riêng của bạn để kiểm tra.

Để sao chép các mẫu prompt và tài liệu mẫu vào S3 bucket, hãy chạy lệnh sau:

Mở trình soạn thảo VSCode, sau đó chạy lệnh trong **TERMINAL**.

````bash
cd ~/environment/bedrock-serverless-workshop
mkdir sample-documents
curl https://www.federalreserve.gov/monetarypolicy/files/monetary20240501a1.pdf --output sample-documents/monetary20240501a1.pdf
curl https://www.federalreserve.gov/monetarypolicy/files/monetary20240918a1.pdf --output sample-documents/monetary20240918a1.pdf
curl https://sustainability.aboutamazon.com/content/dam/sustainability-marketing-site/pdfs/reports-docs/2023-amazon-sustainability-report.pdf --output sample-documents/2023-sustainability-report-amazon.pdf
curl https://investor.henryschein.com/static-files/bcc116aa-a576-4756-a722-90f5e2e22114 --output sample-documents/2023-hs1-10k.pdf

````

## Tải lên tài liệu mẫu và prompt templates
Để sao chép các mẫu prompt và tài liệu mẫu bạn vừa tải về lên S3 bucket, chạy lệnh sau:
````bash
cd ~/environment/bedrock-serverless-workshop
aws s3 cp sample-documents s3://$S3BucketName/sample-documents/ --recursive
aws s3 cp prompt-engineering s3://$S3BucketName/prompt-engineering/ --recursive
````
Sau khi tải lên thành công, mở AWS S3 Console và truy cập bucket. Bạn sẽ thấy giao diện tương tự như sau:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/003.png?raw=true)

## Lập chỉ mục tài liệu mẫu bằng Amazon Kendra

Chỉ mục Amazon Kendra và nguồn dữ liệu từ Amazon S3 đã được tạo sẵn trong quá trình thiết lập ban đầu của workshop này. Trong bước này, bạn sẽ lập chỉ mục toàn bộ tài liệu trong thư mục sample-documents trên S3.
1. [Mở bảng điều khiển Amazon Kendra](https://console.aws.amazon.com/kendra/)

2. Ở góc trên bên trái giao diện Kendra, chọn biểu tượng menu ☰, sau đó trong bảng điều hướng chọn Indexes.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/001.png?raw=true)



![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/002.png?raw=true)

3. Nhấp vào tên của chỉ mục (index) để mở bảng điều hướng bên trái.

4. Để bắt đầu lập chỉ mục toàn bộ tài liệu từ thư mục sample-documents, chọn S3DocsDataSource, sau đó chọn Sync now. Quá trình lập chỉ mục có thể mất vài phút. Vui lòng chờ cho đến khi hoàn tất.


![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/004.png?raw=true)
5. Để gửi truy vấn đến chỉ mục Amazon Kendra với một vài câu hỏi mẫu, trong bảng điều hướng bên trái, chọn Search indexed content, sau đó nhập câu hỏi.

Ví dụ câu hỏi:
````bash
What is federal funds rate as of May 2024??
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/005.png?raw=true)

--- 
### Xin chúc mừng
Bạn đã hoàn thành nhiệm vụ và có thể chuyển sang bước tiếp theo.