---
title : "Kỹ thuật thiết kế Prompt"
date : 2025-07-17
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

Kỹ thuật thiết kế prompt (prompt engineering) là một yếu tố then chốt trong kiến trúc Retrieval-Augmented Generation (RAG), vì nó đóng vai trò quan trọng trong từng giai đoạn của quy trình.  

Trong giai đoạn Truy xuất (Retrieval), prompt engineering được sử dụng để tạo ra các truy vấn hiệu quả nhằm tìm kiếm thông tin liên quan nhất từ các nguồn dữ liệu, chẳng hạn như tài liệu Amazon S3 đã được lập chỉ mục bởi Amazon Kendra, đồng thời phản ánh chính xác ý định của người dùng.  

Trong giai đoạn Phân tích (Analysis), prompt giúp định hướng việc xử lý và trích xuất thông tin chi tiết từ dữ liệu đã truy xuất, bằng cách xác định loại phân tích, mức độ chi tiết và định dạng kết quả mong muốn.  

Cuối cùng, trong giai đoạn Sinh nội dung (Generation), prompt engineering là yếu tố thiết yếu để khai thác tối đa sức mạnh của các mô hình ngôn ngữ lớn như Claude 3 Sonnet nhằm tạo ra các phản hồi mạch lạc, phù hợp với ngữ cảnh và cụ thể theo nhiệm vụ, cung cấp đầy đủ ngữ cảnh, hướng dẫn và nguyên tắc cần thiết.  

Bằng cách thành thạo kỹ thuật thiết kế prompt, người dùng có thể tối ưu hiệu suất của kiến trúc RAG, đảm bảo rằng thông tin được truy xuất là chính xác, phân tích có chiều sâu và phản hồi được sinh ra giải quyết đúng nhu cầu của người dùng.

Trong tác vụ này, bạn sẽ tinh chỉnh prompt dành cho mô hình ngôn ngữ lớn Claude 3 Sonnet. Bạn sẽ thử nghiệm xem phản hồi thay đổi như thế nào khi bạn cung cấp các mẫu prompt khác nhau.  

Ban đầu, bạn sẽ đặt một câu hỏi với prompt có cấu trúc lỏng lẻo, điều này có thể khiến mô hình LLM đi chệch khỏi ngữ cảnh và tạo ra phản hồi không như mong muốn. Khi bạn dần cải thiện prompt, phản hồi sẽ ngày càng bám sát kết quả mong đợi và giảm thiểu hiện tượng "ảo giác" (hallucination).  

Phần này vẫn thuộc kiến trúc RAG và sẽ sử dụng cùng nguồn dữ liệu như trong tác vụ trước.

1. Quay lại trình duyệt và mở trang web chatbot. Nhấp vào liên kết **Prompt Engineering**. Giao diện trang web sẽ hiển thị như sau:  
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/010.png?raw=true)

2. Thử đặt một câu hỏi không liên quan đến các tài liệu nguồn mà bạn đã lập chỉ mục trước đó.

````bash
Tell me a story about a fox and tiger, the story must be for a 5 year old and under 100 words.
````
3. Đối với mẫu prompt, sao chép đoạn prompt sau và dán vào ô **Prompt Template**. Đây là một prompt có cấu trúc lỏng lẻo, không cung cấp bất kỳ hướng dẫn hay chỉ dẫn cụ thể nào cho mô hình Claude 3.5 Sonnet.

````bash
 {context} and {question}
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/011.png?raw=true)

4. Bạn rất có thể sẽ nhận được một phản hồi tương tự như hình bên dưới. Tuy nhiên, phản hồi này không hề tồn tại trong bất kỳ tài liệu nguồn nào. Do cấu trúc prompt yếu, mô hình đã tạo ra phản hồi "ảo" (hallucinated), tức là một nội dung không dựa trên ngữ cảnh hoặc thông tin đã được cung cấp.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/015.png?raw=true)

5. Bây giờ, hãy cập nhật prompt với phiên bản đã được tinh chỉnh bên dưới. Với prompt đã được cải thiện này, bạn có thể kỳ vọng nhận được phản hồi đúng như mong đợi.


````bash
Human: You are an intelligent AI advisor, and provide answers to questions by using fact based information. 
Use the following pieces of information to provide a concise answer to the question enclosed in <question> tags. 
Look for the contextual information enclosed in <context> tags.
If you don't know the answer, just say that you don't know, don't try to make up an answer.
<context>{context}</context>

<question>{question}</question>

The response should be specific and use facts only.

Assistant:
````
6. Quan sát phản hồi. Lần này, thay vì tạo ra thông tin "ảo", mô hình đã nhận biết rằng không có ngữ cảnh liên quan nào để đưa ra câu trả lời. Điều này chủ yếu là nhờ vào việc prompt đã được chỉ dẫn rõ ràng và cụ thể, từ đó dẫn đến kết quả đúng như mong đợi.

![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/013.png?raw=true)

7. Cuối cùng, hãy thử đặt một câu hỏi có liên quan đến tập kiến thức của bạn và quan sát phản hồi. Phản hồi trông có tính chính xác cao hơn và được trình bày rõ ràng hơn.

````bash
 What is federal funds rate as of May 2024?
````
![ConnectPrivate](https://github.com/PVinhP/PPV_Workshop_01/blob/main/Workshop/static/images/5.fwd/task5/014.png?raw=true)

Bạn có thể thử nghiệm với các câu hỏi và prompt khác nhau cho đến khi nhận được các phản hồi ngắn gọn và nhất quán. Kỹ thuật thiết kế prompt là một quá trình lặp đi lặp lại, và bạn có thể sẽ cần thử nhiều lựa chọn khác nhau cho đến khi phản hồi phù hợp với tiêu chí mong muốn của bạn.
