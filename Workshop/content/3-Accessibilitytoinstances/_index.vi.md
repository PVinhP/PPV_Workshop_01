---
title: "Triển khai Ứng dụng Serverless Amazon Bedrock"
date: 2025-06-17
weight: 3
chapter: false
pre: " <b> 3. </b> "
---
Trong nhiệm vụ này, bạn sẽ xây dựng và triển khai cả hai thành phần backend và frontend của ứng dụng. Backend được triển khai dưới dạng ứng dụng serverless bằng AWS SAM, tạo ra một Amazon API Gateway, các hàm AWS Lambda cần thiết, một nhóm người dùng Cognito và lưu trữ thông tin đăng nhập trong AWS Secrets Manager.

Frontend được xây dựng bằng Vue.js và triển khai bằng AWS Amplify. Giao diện người dùng frontend sẽ gọi các API dựa trên REST được lưu trữ bằng Amazon API Gateway, API Gateway gọi hàm AWS Lambda, hàm này gọi các API Amazon Bedrock. Kiến trúc serverless này cho phép ứng dụng mở rộng hiệu quả và giảm thiểu chi phí vận hành quản lý cơ sở hạ tầng bên dưới. Quá trình triển khai được tự động hóa thông qua một script startup.sh, xử lý toàn bộ việc cung cấp toàn bộ ngăn xếp ứng dụng từ đầu đến cuối.

### Xây dựng và triển khai ứng dụng chatbot
Từ trình chỉnh sửa VSCode, chạy lệnh sau để thiết lập một biến môi trường chính được sử dụng trong suốt hội thảo. Thay thế `<chatbot-startup-stack>` bằng tên ngăn xếp thực tế từ bảng điều khiển AWS CloudFormation.

Biến CFNStackName sẽ được tham chiếu sau này khi tương tác với các tài nguyên được cung cấp như một phần của triển khai ứng dụng. Và biến môi trường S3BucketName sẽ được sử dụng trong suốt hội thảo để lưu trữ các nguồn dữ liệu và cơ sở tri thức cần thiết cho ứng dụng. Các dịch vụ backend sẽ tương tác với nội dung của bucket S3 này để truy xuất và xử lý thông tin cần thiết cho chức năng của ứng dụng. Việc đảm bảo biến môi trường S3BucketName được thiết lập đúng sẽ cho phép các nhiệm vụ hội thảo truy cập và sử dụng liền mạch dữ liệu cần thiết được lưu trữ tại vị trí trung tâm này.


````bash
  export CFNStackName="chatbot-startup-stack"
  export S3BucketName=$(aws cloudformation describe-stacks --stack-name ${CFNStackName} --query "Stacks[0].Outputs[?OutputKey=='S3BucketName'].OutputValue" --output text)
  echo "S3 bucket name: $S3BucketName"

````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh8.png?raw=true)

Để sao chép source code cho workshop này, chạy lệnh sau. Code ứng dụng được lưu trữ trong kho GitHub aws-samples open-source, chứa nhiều dự án mẫu do AWS cung cấp. Việc sao chép kho sẽ đảm bảo bạn có phiên bản mới nhất của mã và có thể dễ dàng thực hiện bất kỳ sửa đổi nào khi tiến hành các nhiệm vụ workshop.

````bash
cd ~/environment
git clone https://github.com/aws-samples/bedrock-serverless-workshop.git
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh9.png?raw=true)
Để build và triển khai backend và frontend của ứng dụng serverless, chạy lệnh sau. Script aws-creds.py được sử dụng để tạo một AWS profile, đây là bước cần thiết cho quá trình triển khai phần frontend sau đó.

````bash
cd ~/environment/bedrock-serverless-workshop
python3 aws-creds.py
chmod +x startup.sh
./startup.sh
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh11.png?raw=true)
Script sẽ thực hiện các bước sau:

1. Nâng cấp hệ điều hành, cài đặt phần mềm jq.

2. Build backend bằng sam build.

3. Triển khai backend bằng sam deploy.

4. Cài đặt Amplify và build frontend.

5. Triển khai ứng dụng frontend thông qua Amplify.

Script sẽ mất từ 2 đến 5 phút để hoàn tất.

{{% notice note %}}
Nếu có cửa sổ popup cảnh báo từ git, hãy chọn OK.
{{% /notice %}}

{{% notice note %}}
Khi chạy amplify add host, bạn sẽ được yêu cầu chọn **hai lần**. Giữ nguyên các tùy chọn mặc định và nhấn **Enter**:
Hosting with Amplify Console (Managed hosting with custom domains, Continuous deployment) và Manual deployment
{{% /notice %}}

Sau khi hoàn tất, bạn sẽ thấy hình ảnh sau:

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh12.png?raw=true)

---
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh13.png?raw=true)

---
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh14.png?raw=true)

---
Sao chép và lưu lại giá trị của địa chỉ **amplifyapp url**, **user_id** và **password** vào một trình soạn thảo văn bản. Bạn sẽ sử dụng các thông tin này để đăng nhập vào giao diện người dùng.

Mở địa chỉ **amplifyapp url** trên trình duyệt web và đăng nhập bằng thông tin ở trên. Sau khi đăng nhập thành công, bạn sẽ thấy màn hình chính như bên dưới. Lưu ý, hiện tại hệ thống vẫn chưa sẵn sàng với các tài liệu nguồn và tính năng chat chưa hoạt động. Trong tác vụ tiếp theo, bạn sẽ hoàn tất việc lập chỉ mục tài liệu nguồn và kiểm thử với các câu hỏi mẫu.
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh15.png?raw=true)

---
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh16.png?raw=true)

---
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/anh/anh17.png?raw=true)
Trước khi chuyển sang tác vụ tiếp theo, chạy các lệnh bên dưới — đây là các biến môi trường cần thiết cho các lệnh SAM và Amplify build trong phần còn lại của lab.

Chạy các lệnh sau trong terminal của VS Code:

````bash
export SAMStackName="sam-$CFNStackName"
export AWS_REGION=$(aws configure get region)
export KendraIndexID=$(aws cloudformation describe-stacks --stack-name ${CFNStackName} --query "Stacks[0].Outputs[?OutputKey=='KendraIndexID'].OutputValue" --output text)
export BedrockApiUrl=$(aws cloudformation describe-stacks --stack-name ${SAMStackName} --query "Stacks[0].Outputs[?OutputKey=='BedrockApiUrl'].OutputValue" --output text)
export SecretName=$(aws cloudformation describe-stacks --stack-name ${SAMStackName} --query "Stacks[0].Outputs[?OutputKey=='SecretsName'].OutputValue" --output text)
export BedrockApiUrl=$(aws cloudformation describe-stacks --stack-name ${SAMStackName} --query "Stacks[0].Outputs[?OutputKey=='BedrockApiUrl'].OutputValue" --output text)
export UserPoolId=$(aws cloudformation describe-stacks --stack-name ${SAMStackName} --query "Stacks[0].Outputs[?OutputKey=='CognitoUserPool'].OutputValue" --output text)
export UserPoolClientId=$(aws cloudformation describe-stacks --stack-name ${SAMStackName} --query "Stacks[0].Outputs[?OutputKey=='CongnitoUserPoolClientID'].OutputValue" --output text)
export SecretName=$(aws cloudformation describe-stacks --stack-name ${SAMStackName} --query "Stacks[0].Outputs[?OutputKey=='SecretsName'].OutputValue" --output text)
export PATH=~/.npm-global/bin:$PATH
````
Sau khi chạy, bạn có thể kiểm tra lại bằng lệnh export và xác minh xem các biến như AWS_REGION, KendraIndexID, BedrockApiUrl, ... đã được gán giá trị chưa.

{{% notice note %}}
Nếu các biến chưa có giá trị, hãy kiểm tra lại vùng (region) của bạn để đảm bảo đang đặt là us-west-2.
Nếu chưa đúng, hãy xóa stack, chọn lại vùng phù hợp và thực hiện lại các bước.
{{% /notice %}}

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/3.connect/001.png?raw=true)
You have completed the deployment of the backend and frontend of the chatbot using Amazon Bedrock serverless