---
title: "Điều kiện tiên quyết"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---


## Thiết lập Tài khoản AWS

Bạn cần một tài khoản AWS có quyền hạn thích hợp để tạo các tài nguyên API Gateway, Lambda, và DynamoDB.

### Quyền hạn bắt buộc
- API Gateway: Tạo và quản lý REST API
- Lambda: Tạo và quản lý các hàm
- DynamoDB: Tạo và quản lý bảng
- IAM: Tạo vai trò và chính sách
- CloudWatch: Xem logs

{{% notice info %}}
Nếu bạn chưa có tài khoản AWS, hãy đăng ký [AWS Free Tier](https://aws.amazon.com/free/) để bắt đầu. Phần lớn workshop này nằm trong giới hạn free tier.
{{% /notice %}}

---

## Yêu cầu địa phương

### Bắt buộc
- **AWS CLI**: Phiên bản 2.x trở lên
  - [Hướng dẫn cài đặt](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
  - Kiểm tra: `aws --version`
  
- **AWS Account Credentials**: Được cấu hình cục bộ
  - Chạy: `aws configure`
  - Nhập AWS Access Key ID và Secret Access Key
  - Khu vực mặc định: `us-east-1` (được khuyến bao)

- **Text Editor hoặc IDE**: VS Code, PyCharm, hoặc tương tự

### Tùy chọn nhưng được khuyên dùng
- **Python 3.8+**: Để kiểm tra các hàm Lambda cục bộ
- **Postman** hoặc **curl**: Để kiểm tra các điểm cuối API
- **git**: Để kiểm soát phiên bản

---

## Dịch vụ AWS được sử dụng trong Workshop

1. **API Gateway** - Các điểm cuối REST API
2. **Lambda** - Máy tính serverless cho logic kinh doanh
3. **DynamoDB** - Cơ sở dữ liệu NoSQL để lưu trữ dữ liệu

### Dịch vụ bổ sung
- **IAM**: Để kiểm soát truy cập và bảo mật
- **CloudWatch**: Để giám sát và logs
- **AWS CloudFormation** (tùy chọn): Cho Infrastructure as Code

---

## Chi phí dự kiến

Workshop này sẽ **tốn kém dưới $1 USD** nếu:
- Hoàn thành trong 2-3 giờ
- Tài nguyên bị xóa sau khi hoàn thành
- Bạn nằm trong giới hạn AWS Free Tier

### Phạm vi miễn phí (12 tháng)
- Lambda: 1.000.000 yêu cầu miễn phí/tháng
- DynamoDB: 25 GB lưu trữ, 25 RCU, 25 WCU
- API Gateway: 1.000.000 lệnh gọi API/tháng (năm đầu tiên)

**Quan trọng**: Luôn xóa tài nguyên sau khi kiểm tra để tránh các khoản phí không mong muốn.

---

## Thời gian cần thiết

**Tổng cộng: 2-3 giờ**
- Thiết lập & Điều kiện tiên quyết: 15-20 phút
- Tạo bảng DynamoDB: 10 phút
- Viết Lambda Functions: 40-50 phút
- Thiết lập API Gateway: 30-40 phút
- Kiểm tra & Gỡ lỗi: 20-30 phút
- Dọn dẹp: 10 phút

---

## Các bước tiếp theo

1. Tạo tài khoản AWS hoặc đảm bảo tài khoản hiện tại của bạn có các quyền hạn cần thiết
2. Cài đặt và cấu hình AWS CLI
3. Kiểm tra thiết lập của bạn bằng: `aws s3 ls`
4. Tiến hành [Tạo bảng DynamoDB](../5.3-S3-vpc/)

---

{{% notice warning %}}
KHÔNG cam kết các thông tin đăng nhập AWS cho kiểm soát phiên bản. Thay vào đó, hãy sử dụng các tệp cấu hình AWS CLI trong `~/.aws/credentials`.
{{% /notice %}}


