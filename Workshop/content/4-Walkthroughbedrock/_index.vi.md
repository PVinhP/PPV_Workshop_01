---
title : "Hướng dẫn sử dụng Amazon Bedrock Console"
date : 2025-06-17
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

Sử dụng Amazon Bedrock để xây dựng và mở rộng các ứng dụng AI tạo sinh (generative AI) với các mô hình nền (Foundation Models - FMs). Amazon Bedrock là một dịch vụ được quản lý toàn phần, cung cấp quyền truy cập vào các mô hình nền từ các startup AI hàng đầu cũng như từ Amazon thông qua API. Bạn có thể lựa chọn từ nhiều mô hình khác nhau để tìm ra mô hình phù hợp nhất với trường hợp sử dụng của mình. Với trải nghiệm serverless của Amazon Bedrock, bạn có thể bắt đầu nhanh chóng, tùy chỉnh riêng các mô hình bằng dữ liệu của bạn, và dễ dàng tích hợp cũng như triển khai vào các ứng dụng của mình bằng các công cụ AWS mà không cần quản lý hạ tầng.

1. Mở [Bảng điều khiển Amazon Bedrock](https://console.aws.amazon.com/bedrock/home)  
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh19.png?raw=true)

#### Kích hoạt quyền truy cập mô hình

Để hoàn thành workshop này, bạn cần bật quyền truy cập các mô hình Claude3, Mistral và Llama. Ngoài ra, hãy kiểm tra xem mô hình **Titan Text Embeddings V2** đã được bật chưa; mô hình này thường được bật mặc định, nhưng hãy xác nhận lại trạng thái và kích hoạt nếu cần. Hãy đảm bảo rằng bạn đã có quyền truy cập tất cả các mô hình cần thiết trước khi tiếp tục.

1. Ở góc trên bên trái của bảng điều khiển Amazon Bedrock, chọn biểu tượng menu (ba dấu gạch ngang). Trong bảng điều hướng, chọn **Model access**, sau đó chọn **Enable specific models**.
2. Chọn các mô hình ngôn ngữ sau:
   - Titan Text Embeddings V2  
   - Claude 3.5 Sonnet  
   - Claude 3 Haiku  
   - Claude 3 Opus  
   - Llama 3.1 8B Instruct  
   - Mistral 7B Instruct  
3. Chọn `Next`  
4. Chọn `Submit`

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh20.png?raw=true)

{{% notice note %}}

Trong môi trường workshop hiện tại, mô hình Claude 3 Opus chưa được hỗ trợ. Nếu bạn gặp lỗi liên quan đến Opus, có thể bỏ qua và tiếp tục mà không cần thử nghiệm với mô hình này. Tuy nhiên, nếu bạn triển khai ứng dụng này trong tài khoản AWS của riêng mình, bạn sẽ có thể kích hoạt mô hình Opus mà không gặp vấn đề gì.

{{% /notice %}}

### Đặt câu hỏi

1. Ở góc trên bên trái của bảng điều khiển Amazon Bedrock, chọn biểu tượng menu (ba dấu gạch ngang).  
   Trong mục **Test**, chọn **Chat / Text playground**.

   Bạn cũng có thể dành thời gian khám phá các tùy chọn khác trong menu bên trái.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/4.Activatemodelaccess/011.png?raw=true)

2. Trong **Chat / Text playground**, chọn **`Select model`**.  
3. Trong cửa sổ *Select model*, chọn **Anthropic** và chọn **Claude 3 Haiku**.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/4.Activatemodelaccess/002.png?raw=true)

4. Chọn **`Apply`**.

5. Trong hộp văn bản nhập prompt, nhập câu hỏi, sau đó chọn **`Run`**.

   Ví dụ câu hỏi – `List top 10 cities of VietNam by population?`

6. Mô hình sẽ trả về câu trả lời tương tự như sau:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/4.Activatemodelaccess/402.png?raw=true)

**Trong phần Cấu hình (Configurations)**,  
Bạn có thể điều chỉnh độ ngẫu nhiên bằng cách thay đổi các tham số như temperature, Top P, Top K và độ dài token. Bạn cũng có thể thử các câu hỏi khác nhau để trải nghiệm phản hồi từ mô hình.

- **Temperature** là tham số điều chỉnh mức độ ngẫu nhiên trong câu trả lời; giá trị càng cao thì phản hồi càng sáng tạo.  
- **Top P** là ngưỡng xác suất tích lũy dùng để chọn các token ứng viên, giúp cân bằng giữa logic và đa dạng.  
- **Top K** là số lượng token có xác suất cao nhất được xem xét để chọn cho từ tiếp theo.  
- **Max token length** là số token tối đa mà mô hình sẽ sinh ra trong một phản hồi, giúp kiểm soát độ dài và chi phí đầu ra.

---

## 🎉 Xin chúc mừng

Bạn đã sẵn sàng để chuyển sang bước tiếp theo.
