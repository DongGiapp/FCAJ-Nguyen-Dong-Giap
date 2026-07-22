---
title: "Nhật ký công việc"
date: 2026-07-22
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

# Nhật ký thực tập

## Phạm vi và tiêu chuẩn bằng chứng

Worklog ghi lại 12 tuần học AWS, thực hành serverless API, viết tài liệu kỹ thuật, kiểm thử và tham gia cộng đồng. Mỗi tuần có bản tiếng Anh tương ứng, gồm mục tiêu, hoạt động đã hoàn thành, kết quả và phần tự đánh giá. Tuyên bố về triển khai hoặc chức năng cần được hỗ trợ bởi artifact trong repository, output lệnh, kết quả test, CloudWatch record hoặc screenshot đã che dữ liệu nhạy cảm.

{{% notice info %}}
Học một dịch vụ AWS, triển khai một chức năng và vận hành chức năng đó trên production là ba cấp độ bằng chứng khác nhau. Worklog sử dụng các thuật ngữ này một cách có chủ đích.
{{% /notice %}}

## Các mốc theo tuần

| Tuần | Trọng tâm | Deliverable hoặc kết quả |
|---:|---|---|
| [1](1.1-week1/) | AWS account và IAM | Ghi nhận thiết lập tài khoản và nguyên tắc least privilege |
| [2](1.2-week2/) | VPC, EC2 và S3 | Ghi chú nền tảng hạ tầng và bài thực hành |
| [3](1.3-week3/) | AWS CLI và DynamoDB | Thực hành CLI và đánh giá data store ban đầu |
| [4](1.4-week4/) | Proposal và giả định chi phí | Phạm vi, kiến trúc, rủi ro và estimate ban đầu |
| [5](1.5-week5/) | Môi trường phát triển Lambda | Chuẩn bị Python handler và local development |
| [6](1.6-week6/) | AWS SAM và Infrastructure as Code | Học SAM/CloudFormation; vẫn cần artifact trong repository để chứng minh production |
| [7](1.7-week7/) | Lambda CRUD handlers | Tài liệu và test cấp workshop cho create, list, update, delete |
| [8](1.8-week8/) | Cognito authentication | Học identity; bằng chứng triển khai authorization vẫn là production gap |
| [9](1.9-week9/) | Deploy AWS và CloudWatch | Kiểm thử API, xem log và metric cơ bản |
| [10](1.10-week10/) | Retest và dịch thuật kỹ thuật | Regression check API và bài dịch cloud |
| [11](1.11-week11/) | Workshop và sự kiện | Hướng dẫn có thể thực hiện lại và ghi nhận học tập cộng đồng |
| [12](1.12-week12/) | Cleanup và review cuối | Dọn tài nguyên, hoàn thiện báo cáo và đánh giá architecture gap |

## Nhóm công việc

- **Cloud fundamentals:** IAM, VPC, EC2, S3, AWS CLI và nhận thức chi phí.
- **Serverless engineering:** API Gateway, Lambda, DynamoDB, khái niệm Cognito, CloudWatch và SAM.
- **Quality and operations:** API testing, troubleshooting, cleanup, documentation và production-readiness review.
- **Phát triển chuyên môn:** Dịch thuật kỹ thuật, tham gia sự kiện, tự đánh giá và giao tiếp song ngữ.

## Truy vết dành cho reviewer

Các trang theo tuần là narrative record. Trang [Kiến trúc giải pháp](../2-Proposal/) ghi quyết định thiết kế và rủi ro chưa xử lý. Phần [Workshop](../5-Workshop/) mô tả quy trình triển khai. Mọi tuyên bố production vẫn có điều kiện cho đến khi checklist readiness có bằng chứng.
