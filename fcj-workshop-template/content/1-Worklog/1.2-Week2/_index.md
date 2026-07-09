---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Học VPC, EC2, S3 — các dịch vụ em nghe nhiều nhất khi mới vào AWS.
* Thực hành tạo EC2 và SSH lần đầu (em hơi lo trước khi làm).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học *Networking Essentials with Amazon VPC* <br> - Ghi chú: VPC, subnet, security group (lúc đầu em hay nhầm SG với NACL) | 27/04/2026 | 27/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Học *Compute Essentials with EC2* <br> - Tìm hiểu instance type, AMI — chọn t3.micro vì Free Tier | 28/04/2026 | 28/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Học *Instance Profiling with IAM Roles for EC2* <br> - Hiểu vì sao không nên nhét access key thẳng vào EC2 | 29/04/2026 | 29/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - **Thực hành:** Tạo EC2 Linux, tạo key pair, SSH vào instance <br> - Em bị lỗi timeout lần đầu vì security group chưa mở port 22 | 30/04/2026 | 30/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Học *Static Website Hosting with Amazon S3* <br> - Thử bật static hosting, upload file HTML đơn giản | 01/05/2026 | 01/05/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được tuần 2:

* Tạo được EC2 instance và SSH thành công sau khi sửa security group.
* Hiểu VPC là "mạng riêng" trên AWS, security group như tường lửa cho từng resource.
* Biết cách host file tĩnh trên S3 (chưa làm website đẹp, nhưng chạy được).
* Gặp lỗi SSH timeout giúp em nhớ kiểm tra SG trước khi đổ lỗi cho máy mình.
* Bắt đầu quen giao diện AWS Console hơn tuần 1.
