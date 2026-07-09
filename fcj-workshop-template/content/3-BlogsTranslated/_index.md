---
title: "Các bài blogs đã dịch"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
alwaysopen: true
---

Trong quá trình thực tập, em đã dịch và biên soạn các bài blog AWS sau từ tiếng Anh sang tiếng Việt:

### [Blog 1 - Tối ưu hóa chi phí LLM với Intelligent Prompt Routing trên Amazon Bedrock](3.1-Blog1/)

Bài viết của Gerardo Arroyo (AWS Community Builder) phân tích cơ chế **Intelligent Prompt Routing** trên Amazon Bedrock — giải pháp tự động định tuyến prompt đến mô hình phù hợp (Haiku cho tác vụ đơn giản, Sonnet cho tác vụ phức tạp) nhằm giảm chi phí vận hành LLM trên production. Bài dịch bao gồm phân tích định lượng chi phí, ứng dụng cho kiến trúc Model Agent, và các hạn chế cần lưu ý khi triển khai.

### [Blog 2 - Tối ưu hóa chi phí backend với kiến trúc Serverless và AWS Lambda](3.2-Blog2/)

Bài viết phân tích bài toán chi phí của kiến trúc EC2 truyền thống (trả tiền cho idle time) và cách **AWS Lambda** giải quyết bằng mô hình pay-per-use. Nội dung bao gồm bản chất FaaS, so sánh chi phí EC2 vs Lambda, ứng dụng cho AI Agent/Chatbot với Bedrock, kiến trúc event-driven, và các hạn chế như cold start.

### [Blog 3 - Tại sao Serverless là tương lai: Lợi ích của AWS Lambda](3.3-Blog3/)

Bài viết trình bày bản chất của Serverless, các lợi ích chính của AWS Lambda như pay-as-you-go, auto-scaling, giảm gánh nặng quản trị máy chủ và tăng tốc phát triển sản phẩm. Nội dung cũng phân tích các use case thực tế như xử lý ảnh/video, REST API, cron job, xử lý dữ liệu real-time, đồng thời nêu rõ những giới hạn cần lưu ý như cold start và execution time limit.
