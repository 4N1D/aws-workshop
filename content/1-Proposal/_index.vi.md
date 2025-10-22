---
title : "Proposal"
date :  2025-10-03
weight : 1
chapter : false
pre : " <b> 1. </b> "
---
# MAPVIBE

**Trình gợi ý Ăn uống bằng AI từ Gợi ý & Ngữ cảnh**  
*(Tìm các địa điểm ăn uống gần đây từ các gợi ý ngôn ngữ tự nhiên và ngữ cảnh thời gian thực)*

---

## 1. Tóm tắt điều hành

Hội thảo giới thiệu một trình gợi ý ăn uống sử dụng AI, có khả năng hiểu các gợi ý ngôn ngữ tự nhiên bằng tiếng Việt/tiếng Anh (ví dụ: “đói quá, muốn ăn lẩu cho 4 người dưới 300k gần Thủ Đức”) và trả về các nhà hàng phù hợp tại **Thành phố Hồ Chí Minh**. Nền tảng tích hợp các tín hiệu ngữ cảnh—vị trí, thời gian, thời tiết, số lượng người và lịch sử tìm kiếm—để cá nhân hóa các gợi ý. Được xây dựng trên kiến trúc **AWS serverless**, hệ thống tập trung vào **phản hồi nhanh (<10 giây)**, **độ chính xác cao (≥85% mức độ hài lòng)** và **tối ưu chi phí (<$200 trong 3 tháng)**. Người dùng đã xác thực sẽ nhận được các lợi ích như giảm giá hoặc ưu đãi cá nhân hóa.

---

## 2. Tuyên bố vấn đề

### Vấn đề là gì?

- Người dùng mất thời gian chuyển đổi giữa Foody, Google Maps hoặc GrabFood để quyết định ăn ở đâu.
- Các nền tảng hiện tại không tận dụng ngữ cảnh (thời gian, tâm trạng, thời tiết, ngân sách, số lượng người).
- Thiếu tương tác hội thoại hiểu ngôn ngữ tự nhiên.

### Giải pháp

Hệ thống sử dụng **AWS Bedrock (LLM)** để phân tích ý định của người dùng, ánh xạ các mong muốn và ràng buộc thành các bộ lọc có cấu trúc, sau đó truy xuất và xếp hạng kết quả từ các API **Google Places**, **Foody/ShopeeFood** và **Foursquare**. Giao diện kết hợp hội thoại và biểu mẫu sẽ hỗ trợ người dùng thu hẹp kết quả và khám phá các địa điểm đang thịnh hành.

### Lợi ích và ROI

- **Tốc độ**: Giảm thời gian quyết định từ vài phút xuống vài giây.
- **Tự động hóa**: Không cần lọc thủ công hoặc chuyển đổi ứng dụng.
- **Cá nhân hóa**: Dựa trên ngữ cảnh, học từ hành vi người dùng.
- **Giá trị giáo dục**: Mô hình tham chiếu cho phân tích gợi ý LLM + tích hợp AWS.
- **Hiệu quả chi phí**: Ước tính ~$65/tháng, $200 cho 3 tháng MVP.
- **Hòa vốn**: Giá trị tức thì cho demo/nghiên cứu; tiềm năng thương mại với các quan hệ đối tác.

---

## 3. Kiến trúc giải pháp

### Tổng quan

Gợi ý + Ngữ cảnh → **Trình phân tích ý định (LLM)** → **Truy vấn có cấu trúc** → **Tìm kiếm đa nhà cung cấp** → **Xếp hạng & Bộ nhớ đệm** → **Hiển thị trên giao diện Web** → **Vòng phản hồi**.
![Solution Architecture](/images/architecture.png)

### Các dịch vụ AWS sử dụng

| Dịch vụ                   | Chức năng                              |
|---------------------------|---------------------------------------|
| Amazon API Gateway        | Điểm cuối bảo mật cho giao diện web   |
| AWS Lambda                | Phân tích ý định, gọi API nhà cung cấp, xếp hạng |
| Amazon DynamoDB           | Lưu trữ bộ nhớ đệm truy vấn (TTL)      |
| Amazon S3                 | Nhật ký, xuất dữ liệu, tài sản tĩnh   |
| Amazon Cognito            | Xác thực (email/mạng xã hội) + theo dõi phần thưởng |
| AWS Amplify / CloudFront  | Lưu trữ giao diện                     |
| Amazon Bedrock            | Phân tích ý định & giọng điệu LLM     |
| OpenSearch Serverless *(tùy chọn)* | Thử nghiệm xếp hạng nâng cao   |

### Thiết kế thành phần

- **Giao diện**: Ứng dụng web (Next.js, song ngữ VI/EN, kết hợp hội thoại + biểu mẫu).
- **Thu thập dữ liệu**: Gợi ý, ngữ cảnh và câu trả lời khảo sát đi qua API Gateway.
- **Lưu trữ dữ liệu**: DynamoDB (kết quả lưu trữ), S3 (tài sản + nhật ký).
- **Xử lý dữ liệu**: Lambda thực thi phân tích gợi ý Bedrock + tổng hợp API.
- **Quản lý người dùng**: Cognito (đăng nhập email & mạng xã hội). Người dùng không đăng nhập bị giới hạn bởi hạn ngạch IP.
- **Đầu ra**: Danh sách/bản đồ các địa điểm hàng đầu, với các chỉ số như mở cửa, đánh giá, khoảng cách và phù hợp với tâm trạng.

---

## 4. Triển khai kỹ thuật

### Các giai đoạn triển khai

| Giai đoạn | Mô tả                                          | Thời gian   |
|-----------|------------------------------------------------|-------------|
| 1         | Xác định kiến trúc, API nhà cung cấp, lược đồ gợi ý Bedrock | 2 tuần |
| 2         | Ước tính chi phí và chiến lược lưu trữ         | 1 tuần      |
| 3         | Xây dựng backend (Lambda + DynamoDB + Bedrock) | 3 tuần      |
| 4         | Phát triển giao diện (Next.js giao diện song ngữ) | 3 tuần   |
| 5         | Kiểm thử và tối ưu hóa (<10 giây độ trễ mục tiêu) | 2 tuần  |
| 6         | Ra mắt MVP và thu thập phản hồi người dùng     | 1 tuần      |

### Yêu cầu kỹ thuật

- **Thiết bị đầu cuối**: Trình duyệt người dùng hoặc trình duyệt di động (sẵn sàng PWA).
- **Đám mây**: AWS Bedrock, Lambda, API Gateway, DynamoDB, S3, Amplify, Cognito.
- **Công cụ & Khung**: Next.js (App Router), TypeScript, AWS CDK, GitHub Actions CI/CD, Google Places SDK.

---

## 5. Lộ trình & Mốc thời gian

| Giai đoạn           | Hoạt động                                                  |
|---------------------|------------------------------------------------------------|
| Trước hội thảo (Tháng 0) | Nghiên cứu tập dữ liệu nhà hàng HCM, kiểm thử API         |
| Tháng 1             | Xây dựng MVP backend với trình phân tích ý định Bedrock   |
| Tháng 2             | Kết nối API nhà cung cấp, lưu trữ, tích hợp giao diện     |
| Tháng 3             | Ra mắt bản beta công khai; thu thập phản hồi              |
| Sau hội thảo        | Mở rộng sang xếp hạng lại dựa trên ML, danh sách ngoại tuyến và gợi ý dựa trên sự kiện |

---

## 6. Ước tính ngân sách

### Chi phí cơ sở hạ tầng đám mây

| Dịch vụ AWS           | Chi phí/Tháng (USD) | Mô tả                    |
|-----------------------|--------------------|--------------------------|
| Lambda                | 15                 | API + logic LLM          |
| DynamoDB              | 10                 | Lưu trữ truy vấn bộ nhớ đệm |
| S3                    | 5                  | Nhật ký, tệp tĩnh        |
| API Gateway           | 10                 | Định tuyến yêu cầu       |
| Cognito               | 5                  | Xác thực MAU             |
| Amplify/CloudFront    | 10                 | Lưu trữ/CDN              |
| Bedrock (token LLM)   | 15                 | Phân tích gợi ý          |
| **Tổng cộng**         | **≈ 65/tháng**     | **≈ 200/3 tháng**        |

### API bên ngoài

| Nhà cung cấp        | Gói                 | Ước tính hàng tháng |
|---------------------|---------------------|---------------------|
| Google Places       | Trả theo sử dụng    | 15                  |
| Foody/ShopeeFood    | API miễn phí (giới hạn) | 0               |
| Foursquare          | Gói nhà phát triển  | 5                   |

---

## 7. Đánh giá rủi ro

| Rủi ro                          | Tác động | Xác suất | Giải pháp giảm thiểu                    |
|---------------------------------|----------|----------|----------------------------------------|
| Vượt hạn ngạch hoặc chi phí API  | Cao      | Trung bình | Triển khai bộ nhớ đệm + giới hạn tốc độ |
| Phân tích LLM không chính xác (văn bản VN) | Trung bình | Thấp     | Sử dụng mẫu gợi ý & xác thực sau    |
| Thay đổi API nhà cung cấp       | Trung bình | Trung bình | Bộ điều hợp trừu tượng              |
| Vấn đề quyền riêng tư (đồng ý vị trí) | Cao   | Thấp     | Yêu cầu đồng ý rõ ràng               |

**Kế hoạch dự phòng:**  
Sử dụng kết quả lưu trữ + nhà cung cấp dự phòng. Tệp JSON cục bộ cho demo. Giới hạn lưu lượng không xác thực.

---

## 8. Kết quả mong đợi

### Cải tiến kỹ thuật

- Tìm kiếm hội thoại (VN/EN) với độ trễ <10 giây.
- Lược đồ có cấu trúc để phân tích ý định ăn uống.
- Xếp hạng thống nhất theo khoảng cách, đánh giá và phù hợp ngữ cảnh.

### Giá trị dài hạn

- Mở rộng sang xếp hạng lại dựa trên ML & cá nhân hóa.
- PWA danh sách ngắn cho quyết định ngoại tuyến.
- Tích hợp với API đặt chỗ hoặc giao hàng.
- Mở rộng tương lai: gợi ý dựa trên **thời tiết/sự kiện đặc biệt** hoặc **tâm trạng xã hội**.

---

### Tài liệu đính kèm / Tham khảo

- **Liên kết AWS Pricing Calculator**: [Chưa xác định]
- **Sơ đồ kiến trúc**: [Đang chờ bản nháp]
- **Kho GitHub**: [Đang chờ thiết lập]
- **Bảng ngân sách**: [Đang tiến hành]