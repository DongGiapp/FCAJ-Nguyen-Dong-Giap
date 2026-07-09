---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

**Bài gốc (tiếng Anh):** [Stop Paying for Every Token: Amazon Bedrock Intelligent Prompt Routing](https://towardsaws.com/stop-paying-for-every-token-amazon-bedrock-intelligent-prompt-routing-f01d81a7e18f)  
**Tác giả:** Gerardo Arroyo (AWS Community Builder)  
**Người dịch:** Nguyễn Đông Giáp

# Tối ưu hóa chi phí vận hành LLM qua cơ chế Định tuyến Prompt Thông minh (Intelligent Prompt Routing)

Một "bẫy chi phí" phổ biến mà nhiều team AI gặp phải khi đưa hệ thống lên production là chiến lược **Một Mô Hình Duy Nhất (Single-Model Approach)**. Việc sử dụng các mô hình lớp trên (như Claude 3.5 Sonnet) cho mọi tác vụ — từ câu hỏi định nghĩa đơn giản đến phân tích kiến trúc phức tạp — đang tạo ra sự lãng phí tài nguyên biên rất lớn.

Để giải quyết bài toán mất cân bằng này, giải pháp **Intelligent Prompt Routing** trên Amazon Bedrock cung cấp một cách tiếp cận mang tính hệ thống dựa trên việc phân phối tải động theo thời gian thực. Dưới đây là phân tích chi tiết về cơ chế, hiệu quả định lượng và góc nhìn ứng dụng cho các Model Agent.

![Intelligent Prompt Routing - How Routing Works](/images/3-BlogsTranslated/3.1-Blog1/intelligent-prompt-routing.png)

---

## 1. Bản chất kỹ thuật của Intelligent Prompt Routing

Thay vì yêu cầu kỹ sư phải thiết lập cứng (hard-code) hoặc tự xây dựng các mô hình phân loại phụ (classification models) tốn kém để phân bổ prompt, giải pháp này cung cấp một **Serverless Endpoint** duy nhất làm trung gian.

Khi một truy vấn đầu vào (*Incoming Prompt*) đi vào hệ thống, Router sẽ chạy một thuật toán dự đoán nội bộ để ước lượng độ phức tạp và hiệu suất mong đợi của từng phân lớp mô hình trong cùng một dòng (*Model Family*) theo thời gian thực:

| Loại tác vụ | Mô tả | Mô hình được định tuyến |
| --- | --- | --- |
| **High Complexity / Deep Reasoning** | Tác vụ phức tạp, cần suy luận sâu | Claude 3.5 Sonnet, Llama 3.1 70B |
| **Low Complexity / Operational** | Tra cứu thông tin, FAQ, tác vụ thủ tục | Claude 3 Haiku, Llama 3.1 8B |

Luồng xử lý: **User Query → API Gateway → Lambda → Amazon Bedrock (Intelligent Prompt Routing) → Nova Pro / Nova Lite** (hoặc các mô hình tương đương trong cùng model family).

---

## 2. Phân tích định lượng và hiệu quả kinh tế

Dựa trên các thực nghiệm thực tế được ghi nhận bởi AWS Community Builder cho một truy vấn đơn giản (ước tính khoảng 15 input tokens và 300 output tokens), hiệu quả tối ưu hóa được thể hiện rất rõ ràng:

### Chi phí và độ trễ trên mỗi truy vấn đơn lẻ

| Mô hình | Chi phí / query | Độ trễ trung bình |
| --- | --- | --- |
| Claude 3 Haiku | $0.000379 | ~3.7 giây |
| Claude 3.5 Sonnet | $0.0066 | ~9.4 giây |

**Hiệu quả:** Giảm **94.26%** chi phí và tốc độ phản hồi nhanh hơn **~2.5 lần** đối với tác vụ đơn giản.

### Quy mô production (1 triệu truy vấn đơn giản)

| Chiến lược | Chi phí ước tính |
| --- | --- |
| Dùng thuần Sonnet | ~$6,600.00 |
| Dùng thuần Haiku | ~$378.75 |
| **Tiết kiệm tối đa** | **$6,221.25** (giảm ~17.4 lần chi phí biên) |

**Case study thực tế:** Báo cáo từ DoiT cho thấy khi kết hợp chiến lược định tuyến mô hình linh hoạt cùng các giải pháp tối ưu hạ tầng khác, một hệ thống doanh nghiệp lớn đã giảm hóa đơn vận hành Bedrock từ **$40,000/tháng** xuống còn **$18,000/tháng**.

---

## 3. Ứng dụng nâng cao: Tối ưu hóa cho cấu trúc Model Agent

Đối với các hệ thống chạy theo kiến trúc **Model Agent** (Hệ tác nhân AI), cơ chế định tuyến này giải quyết được bài toán thắt nút cổ chai về cả chi phí lẫn thời gian phản hồi qua các vòng lặp tư duy (*Reasoning loops*):

**Phân cấp tác vụ trong Agent Workflow:** Các Agent thường trải qua nhiều bước lặp như Lập kế hoạch (Planning), Gọi công cụ (Tool calling), Trích xuất thực thể (Data Extraction), và Đánh giá (Reflection). Router giúp các bước trung gian mang tính thủ tục chạy trên Haiku để lấy tốc độ và chi phí thấp, chỉ kích hoạt Sonnet khi cần xử lý logic nặng hoặc giải quyết ngoại lệ (Exception handling).

**Giảm chi phí tích lũy bộ nhớ (Memory Accumulation):** Agent bắt buộc phải duy trì lịch sử hội thoại (*Context window*) lớn qua từng bước lặp. Khi kết hợp cơ chế Prompt Routing với **Prompt Caching** (giảm tới 90% chi phí cho các token lặp lại), hệ thống Agent sẽ đạt được trạng thái tối ưu hóa kép (*Double Win*).

---

## 4. Các hạn chế hệ thống cần lưu ý

Mặc dù mang lại hiệu quả kinh tế cao, kiến trúc này vẫn tồn tại những rào cản kỹ thuật ở giai đoạn hiện tại:

- **Giới hạn ngôn ngữ:** Thuật toán phân tích độ phức tạp của Router hiện tại được huấn luyện tối ưu nhất trên tập dữ liệu tiếng Anh. Khi áp dụng cho các ngôn ngữ khác như tiếng Việt, độ chính xác của việc định tuyến có thể bị ảnh hưởng.
- **Tính đóng gói của Router:** Hệ thống phân phối dựa trên thuật toán tổng quát của AWS, không thể tự động tinh chỉnh hoặc học lại dựa trên dữ liệu hiệu năng (*performance data*) đặc thù riêng biệt của từng ứng dụng.

---

## Kết luận

Cơ chế **Định tuyến Prompt Thông minh** dịch chuyển tư duy thiết kế hệ thống AI từ việc tìm kiếm một mô hình vạn năng sang tư duy phối hợp các phân lớp mô hình một cách linh hoạt.

Điểm cộng lớn của giải pháp này là **"zero integration overhead"** — kỹ sư không cần viết thêm pipeline hay logic phân loại phức tạp nào trong code, chỉ cần thay đổi mã định danh mô hình (`modelId`) từ mã ARN của một model cụ thể sang mã ARN của **Prompt Router** là hệ thống có thể tự động vận hành mượt mà.

---

**Nguồn tham khảo:**  
- [Towards AWS — Gerardo Arroyo](https://towardsaws.com/stop-paying-for-every-token-amazon-bedrock-intelligent-prompt-routing-f01d81a7e18f)  
- [Amazon Bedrock Intelligent Prompt Routing (AWS Docs)](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-routing.html)
