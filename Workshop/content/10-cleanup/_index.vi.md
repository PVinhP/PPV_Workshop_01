---
title : "Dọn dẹp tài nguyên"
date :  2025-07-17
weight : 10
chapter : false
pre : " <b> 10. </b> "
---

Sau khi bạn hoàn thành workshop, để tránh phát sinh chi phí, bạn nên xóa các tài nguyên đã tạo trong tài khoản AWS của mình.

Sử dụng AWS CLI để xóa tài nguyên

### Xóa ứng dụng AWS SAM và CloudFormation stack khởi tạo ban đầu

Truy cập terminal của AWS Cloud9, sau đó chạy các lệnh sau:

1. Để xóa SAM stack, chạy lệnh sau:
```bash
cd ~/environment/bedrock-serverless-workshop
sam delete
```
2. Để xóa stack khởi tạo ban đầu, chạy lệnh sau:
```bash
aws cloudformation delete-stack --stack-name $CFNStackName
``` 