---
title : "Chuẩn bị "
date : 2025-07-17
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
Bạn nên sử dụng IAM user với quyền quản trị thay vì tài khoản root. Bởi vì một số dịch vụ AWS, chẳng hạn như Knowledge Bases trong Amazon Bedrock hoặc các dịch vụ liên quan đến AI khác, có thể bị hạn chế hoặc không khả dụng khi sử dụng tài khoản root. Vì lý do bảo mật và tương thích, các best practice của AWS cũng khuyên tránh sử dụng root user cho các tác vụ hàng ngày.
{{% /notice %}}
{{% notice info %}}
**Regions được hỗ trợ**: **us-west-2**.
{{% /notice %}} 
Trong workshop này, bạn sẽ cấu hình các dịch vụ AWS sau để xây dựng một chatbot AI tổng hợp.
 - [Amazon Bedrock](https://aws.amazon.com/vi/bedrock/)
 - [Knowledge Bases ](https://aws.amazon.com/vi/bedrock/knowledge-bases/)
 - [Amazon Kendra](https://aws.amazon.com/vi/kendra/)
 - [AWS Lambda](https://aws.amazon.com/vi/lambda/)

Một template AWS CloudFormation được sử dụng để thiết lập các tài nguyên lab trong AWS Region mà bạn chọn. Bước này là bắt buộc vì các hướng dẫn sau đây dựa trên những tài nguyên này

{{% notice warning %}}
**Chi phí**: Lưu ý rằng nếu bạn chạy workshop này trong tài khoản AWS của riêng mình, bạn sẽ phải chịu chi phí cho việc sử dụng tài nguyên. Phần Clean Up trong bảng điều hướng bên trái có thể giúp bạn loại bỏ các tài nguyên khỏi môi trường của mình khi hoàn thành.
{{% /notice %}}
### Nội dung
 - [2.1 Khởi chạy CloudFormation stack](2.1-launchastack/)
 - [2.2 Khởi chạy VSCode trên AWS và Thiết lập Môi trường](2.2-launchVScode/)