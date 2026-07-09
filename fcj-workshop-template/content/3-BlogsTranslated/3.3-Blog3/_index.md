---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Tại sao Serverless là tương lai: Lợi ích của AWS Lambda

Chào mọi người, chắc hẳn cụm từ **Serverless** hay **AWS Lambda** không còn quá xa lạ trong cộng đồng cloud architecture. Nhưng tại sao xu hướng này lại được coi là tương lai của phát triển phần mềm? Liệu nó có thực sự "thần thánh" như lời đồn?

Bài viết này chia sẻ một góc nhìn thực tế về lý do Serverless, với đại diện tiêu biểu là AWS Lambda, đang thay đổi cách chúng ta xây dựng và vận hành ứng dụng.

![Serverless Image Handler với CloudFront, API Gateway, Lambda và S3](/images/3-BlogsTranslated/3.3-Blog3/serverless-image-handler.png)

---

## 1. Định nghĩa lại Serverless: Bản chất là gì?

Nhiều bạn mới tiếp cận thường thắc mắc: "Serverless là không có máy chủ à?"

Thực tế, máy chủ vẫn tồn tại. Điểm mấu chốt là bạn không cần trực tiếp quản lý nó nữa.

Với mô hình truyền thống như Amazon EC2, đội kỹ thuật thường phải tự:

- Chọn cấu hình máy chủ
- Cài đặt hệ điều hành
- Cấu hình mạng
- Quản lý bảo mật và bản vá
- Theo dõi tài nguyên, lỗi phần cứng và khả năng mở rộng

Với Serverless, AWS quản lý phần hạ tầng bên dưới. Bạn chỉ cần tập trung vào phần tạo ra giá trị cho doanh nghiệp: **code và logic sản phẩm**.

---

## 2. Bốn lợi ích vượt trội của Serverless

### Tiết kiệm chi phí: Pay-as-you-go đến từng mili-giây

Với server truyền thống, dù ứng dụng không có ai truy cập vào ban đêm, doanh nghiệp vẫn phải trả tiền cho thời gian máy chủ chạy không tải.

AWS Lambda giải quyết vấn đề này bằng cơ chế **pay-per-use**:

- Chỉ tính phí khi code thực sự chạy
- Thời gian chạy được tính theo thời lượng thực thi
- Nếu ứng dụng không có lượt truy cập, chi phí compute gần như bằng 0

Đây là lợi thế rất lớn với các hệ thống có lưu lượng không đều, chẳng hạn như website chiến dịch, backend nội bộ, chatbot hoặc ứng dụng mới ra mắt.

### Khả năng mở rộng: Tự động scale theo request

Hãy tưởng tượng hệ thống đang chạy bình thường thì một chiến dịch marketing thành công làm traffic tăng gấp 100 lần.

Nếu dùng EC2, bạn cần thiết lập Auto Scaling Group, cấu hình CloudWatch, kiểm tra load balancer và chờ máy chủ mới khởi động.

Với Lambda, AWS tự động xử lý phần mở rộng:

- 1 request thì Lambda chạy 1 lần
- Nhiều request đồng thời thì Lambda tự mở rộng số execution environment
- Không cần tự quản lý cụm server

Điều này giúp ứng dụng phản ứng nhanh hơn trước các đợt tăng traffic bất ngờ.

### Không còn nỗi lo quản lý máy chủ

Việc bảo trì hệ thống luôn tiêu tốn nhiều thời gian của đội Dev và DevOps. Với Serverless:

- Không cần nâng cấp OS thủ công
- Không phải xử lý lỗi phần cứng
- Không mất nhiều thời gian cấu hình load balancer phức tạp
- Không cần tự duy trì nhiều server chỉ để đảm bảo dự phòng

AWS Lambda được vận hành trên hạ tầng AWS, hỗ trợ tính sẵn sàng cao và khả năng chống chịu lỗi ở cấp dịch vụ.

### Tăng tốc phát triển sản phẩm

Serverless giúp đội phát triển giảm thời gian dành cho hạ tầng và tập trung nhiều hơn vào tính năng.

Thay vì mất nhiều ngày chuẩn bị server, networking, scaling và deployment pipeline phức tạp, nhóm có thể nhanh chóng xây dựng API, xử lý event, tạo automation hoặc triển khai một MVP nhỏ với API Gateway, Lambda, DynamoDB và S3.

---

## 3. Các trường hợp sử dụng thực tế

Serverless rất mạnh, nhưng không phải "viên đạn bạc" cho mọi bài toán. Dưới đây là các kịch bản AWS Lambda phát huy hiệu quả tốt.

| Trường hợp sử dụng | Cách hoạt động thực tế |
| --- | --- |
| Xử lý ảnh/video | User upload ảnh gốc lên S3, Lambda được kích hoạt để resize ảnh, tạo thumbnail và lưu lại vào S3. |
| Xây dựng REST API | Kết hợp API Gateway, Lambda và DynamoDB để tạo backend gọn nhẹ, dễ scale cho app mobile hoặc web. |
| Cron job và tự động hóa | Kết hợp EventBridge với Lambda để dọn dẹp database, gửi email report hoặc chạy tác vụ định kỳ. |
| Xử lý dữ liệu real-time | Xử lý dữ liệu streaming từ IoT device, log hệ thống, Amazon Kinesis hoặc SQS. |

Một ví dụ điển hình là **Serverless Image Handler** của AWS. Kiến trúc này dùng Amazon CloudFront, Amazon API Gateway, AWS Lambda và Amazon S3 để xử lý ảnh theo yêu cầu. Khi ảnh đã được cache ở CloudFront, hệ thống có thể trả ảnh nhanh hơn và tránh xử lý lại không cần thiết.

---

## 4. Góc nhìn thực tế: Serverless có hoàn hảo không?

Để công bằng, khi chọn Lambda, chúng ta cũng cần lưu ý một số thách thức kỹ thuật.

### Cold start

Lần đầu tiên một Lambda function được gọi, hoặc sau một thời gian không dùng, AWS có thể mất một khoảng thời gian ngắn để khởi tạo môi trường thực thi.

Độ trễ này thường từ vài trăm mili-giây đến vài giây tùy runtime, kích thước package và cách khởi tạo dependency.

Có thể tối ưu bằng:

- Provisioned Concurrency
- Giảm kích thước deployment package
- Khởi tạo dependency hợp lý
- Chọn runtime nhẹ như Node.js hoặc Python khi phù hợp

### Execution time limit

Một Lambda function có giới hạn thời gian chạy tối đa 15 phút. Nếu workload cần chạy hàng giờ, chẳng hạn như training AI, render video dài hoặc batch processing rất nặng, EC2, ECS hoặc AWS Fargate thường phù hợp hơn.

### Độ phức tạp khi hệ thống lớn dần

Khi hệ thống có nhiều function, queue, event source và service liên kết với nhau, việc quan sát và debug sẽ khó hơn backend đơn khối.

Vì vậy, kiến trúc Serverless nghiêm túc cần đi kèm logging, monitoring và tracing tốt, ví dụ Amazon CloudWatch và AWS X-Ray.

---

## Kết luận

Serverless không chỉ là một công nghệ, mà là một tư duy kiến trúc mới. Nó giúp startup tối ưu chi phí, giúp developer tập trung vào logic sản phẩm và giúp doanh nghiệp đưa tính năng ra thị trường nhanh hơn.

AWS Lambda đặc biệt phù hợp với các hệ thống có lưu lượng biến động, các tác vụ xử lý theo sự kiện, backend API gọn nhẹ, automation và các sản phẩm cần thử nghiệm nhanh. Tuy vậy, để áp dụng hiệu quả, cần hiểu rõ cả lợi ích lẫn giới hạn như cold start, thời gian chạy tối đa và độ phức tạp vận hành khi hệ thống mở rộng.

---

**Nguồn tham khảo:**  
- [Fast and Cost-Effective Image Manipulation with Serverless Image Handler - AWS Architecture Blog](https://aws.amazon.com/vi/blogs/architecture/fast-and-cost-effective-image-manipulation-with-serverless-image-handler/)
