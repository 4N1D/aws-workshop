---
title: "Đề Xuất"
date: 2025-10-22
weight: 1
chapter: false
pre: " <b> 1. </b> "
---
# MAPVIBE

**Nền Tảng Khám Phá Địa Điểm Dựa Trên Trí Tuệ Nhân Tạo (AI)**  
*(Khám phá các địa điểm ăn uống và giải trí tại Thành phố Hồ Chí Minh bằng các lệnh tự nhiên và thông tin ngữ cảnh)*

---

## 1. Tóm Tắt Thực Hiện

MapVibe là một nền tảng web được điều khiển bởi trí tuệ nhân tạo (AI), được ra mắt tại **Thành phố Hồ Chí Minh** để thay đổi cách khám phá địa điểm, cho phép người dùng tìm kiếm các địa điểm thông qua các lệnh tự nhiên (ví dụ: “tìm một nhà hàng trên sân thượng sang trọng có tầm nhìn thành phố mở cửa đến nửa đêm” hoặc “quán cà phê yên tĩnh gần sông có chỗ ngồi ngoài trời”). Nền tảng này sử dụng **Amazon Bedrock’s Large Language Models (LLMs)** để phân tích ý định của người dùng, tích hợp các yếu tố ngữ cảnh thời gian thực như vị trí, thời gian và sở thích, và lấy dữ liệu từ cơ sở dữ liệu nội bộ **DynamoDB**. Được xây dựng trên **kiến trúc AWS không máy chủ**, MapVibe đảm bảo **độ trễ thấp (<10 giây)**, **độ chính xác cao (≥85% mức độ hài lòng phù hợp)**, và **hiệu quả chi phí (<$200 cho chu kỳ phát triển và trình diễn 8 tuần ban đầu, hoàn thành vào ngày 22 tháng 10 năm 2025)**. Người dùng đã xác thực có thể tận hưởng các đề xuất cá nhân hóa, khả năng đóng góp đánh giá, và truy cập các công cụ kiểm duyệt, tất cả đều được nâng cao bởi các công nghệ AI.

---

## 2. Tuyên Bố Vấn Đề

### Vấn Đề Là Gì?

- Các nền tảng bản đồ truyền thống như Google Maps phụ thuộc vào tìm kiếm dựa trên từ khóa và bộ lọc tĩnh, gặp khó khăn trong việc diễn giải các truy vấn phức tạp và giàu ngữ cảnh (ví dụ: “quán cà phê yên tĩnh gần sông có chỗ ngồi ngoài trời”).
- Người dùng mất thời gian chuyển đổi giữa nhiều ứng dụng để tìm địa điểm ăn uống hoặc hoạt động phù hợp.
- Các giải pháp hiện có thiếu giao diện trò chuyện và không tích hợp các tín hiệu ngữ cảnh như thời gian, tâm trạng hoặc kích thước nhóm.

### Giải Pháp

MapVibe sử dụng **Amazon Bedrock’s LLMs** để phân tích các lệnh tự nhiên bằng tiếng Việt và tiếng Anh, chuyển đổi chúng thành các truy vấn có cấu trúc. Nó lấy và xếp hạng kết quả từ cơ sở dữ liệu nội bộ **DynamoDB** với dữ liệu địa lý được lập chỉ mục, cung cấp giao diện lai (tìm kiếm trò chuyện + bộ lọc danh mục). Nội dung do người dùng tạo (đánh giá, đề xuất địa điểm) được kiểm duyệt bằng **Rekognition**, đảm bảo an toàn và chất lượng thông qua phân tích tiên tiến dựa trên AI.

### Lợi Ích và Tỷ Suất Đầu Tư (ROI)

- **Tốc Độ**: Giảm thời gian khám phá địa điểm từ vài phút xuống còn vài giây.
- **Cá Nhân Hóa**: Kết quả phù hợp với ngữ cảnh dựa trên sở thích và hành vi của người dùng, được hỗ trợ bởi AI.
- **Tự Động Hóa**: Loại bỏ việc lọc thủ công nhờ phân tích ý định dựa trên AI.
- **Khả Năng Mở Rộng**: Cơ sở hạ tầng AWS toàn cầu đảm bảo độ trễ thấp và khả năng phục hồi.
- **Hiệu Quả Chi Phí**: Được tối ưu hóa để nằm trong ngân sách $200 cho chu kỳ 8 tuần ban đầu, hoàn thành vào ngày 22 tháng 10 năm 2025.
- **Tiềm Năng Thương Mại**: Cơ hội hợp tác với các doanh nghiệp địa phương hoặc tích hợp với các hệ thống đặt chỗ nội bộ.

---

## 3. Kiến Trúc Giải Pháp

### Tổng Quan

Lệnh Người Dùng + Ngữ Cảnh → **Bedrock LLM Intent Parsing** → **Structured Query** → **DynamoDB Search** → **Rank & Cache** → **Web UI Display** → **User Feedback Loop**.  
![Solution Architecture](/images/architecture.png)

### Dịch Vụ AWS Được Sử Dụng

| Dịch Vụ                   | Chức Năng                              |
|---------------------------|---------------------------------------|
| Amazon Route 53           | Định tuyến tên miền                    |
| AWS Certificate Manager   | Chứng chỉ SSL/TLS                      |
| AWS WAF                   | Tường lửa ứng dụng web                 |
| Amazon CloudFront         | Mạng phân phối nội dung (CDN) toàn cầu |
| Amazon API Gateway        | Điểm cuối API an toàn                  |
| AWS Lambda                | Xử lý phân tích ý định, tìm kiếm, và xếp hạng |
| Amazon DynamoDB           | Dữ liệu địa lý được lập chỉ mục và bộ nhớ đệm truy vấn |
| Amazon S3                 | Lưu trữ ảnh, nhật ký, và tài sản tĩnh  |
| Amazon Cognito            | Xác thực và ủy quyền người dùng         |
| Amazon Bedrock            | LLM cho phân tích ý định và tóm tắt    |
| Amazon Rekognition        | Kiểm duyệt nội dung dựa trên AI cho các tải lên của người dùng |
| Amazon EventBridge        | Phân tích theo lịch trình và cập nhật huy hiệu |
| Amazon CloudWatch         | Giám sát và ghi nhật ký                |

### Thiết Kế Thành Phần

- **Giao Diện Người Dùng**: Ứng dụng web đáp ứng (Next.js, song ngữ VI/EN, giao diện tìm kiếm lai).
- **Thu Thập Dữ Liệu**: Lệnh và ngữ cảnh được xử lý qua API Gateway; các tải lên của người dùng (đánh giá, ảnh) được kiểm duyệt bởi Rekognition.
- **Lưu Trữ Dữ Liệu**: DynamoDB cho dữ liệu địa điểm và bộ đệm truy vấn (TTL 24 giờ); S3 cho ảnh và nhật ký.
- **Xử Lý Dữ Liệu**: Dịch vụ Lambda xử lý các cuộc gọi Bedrock LLM, thực thi truy vấn, và xếp hạng kết quả.
- **Quản Lý Người Dùng**: Cognito cho xác thực dựa trên JWT (đăng nhập email/xã hội); người dùng chưa đăng nhập có quyền truy cập hạn chế.
- **Đầu Ra**: Hiển thị thẻ địa điểm với tóm tắt do AI tạo, xếp hạng, ảnh, và các nút kêu gọi hành động (CTA) (ví dụ: Lấy Hướng Dẫn, Gọi).

---

## 4. Triển Khai Kỹ Thuật

### Các Giai Đoạn Triển Khai

| Giai Đoạn | Mô Tả                                          | Thời Gian   |
|-----------|------------------------------------------------|------------|
| 1         | Xác định kiến trúc, schema lệnh Bedrock, và schema DynamoDB | Hoàn thành (2 tuần) |
| 2         | Ước tính chi phí và tối ưu hóa chiến lược bộ đệm | Hoàn thành (1 tuần) |
| 3         | Xây dựng backend (Lambda, DynamoDB, Bedrock, Rekognition) | Hoàn thành (3 tuần) |
| 4         | Phát triển giao diện người dùng (Next.js, song ngữ, đáp ứng) | Hoàn thành (3 tuần) |
| 5         | Kiểm tra và tối ưu hóa độ trễ <10 giây và khả năng mở rộng | Hoàn thành (2 tuần) |
| 6         | Ra mắt MVP, triển khai qua CI/CD, thu thập phản hồi | Đang tiến hành (bắt đầu 22/10/2025) |

### Yêu Cầu Kỹ Thuật

- **Thiết Bị Cạnh**: Trình duyệt hiện đại (Chrome, Safari, Firefox) với giao diện đáp ứng sẵn sàng PWA.
- **Đám Mây**: AWS Route 53, ACM, WAF, CloudFront, API Gateway, Lambda, DynamoDB, S3, Cognito, Bedrock, Rekognition, EventBridge, CloudWatch.
- **Công Cụ & Khung**: Next.js (App Router), TypeScript, AWS CDK cho cơ sở hạ tầng dưới dạng mã, GitHub Actions cho CI/CD.

---

## 5. Dòng Thời Gian & Cột Mốc

| Thời Điểm              | Hoạt Động                                                  |
|-------------------------|-------------------------------------------------------------|
| Trước Phát Triển (Tháng 0) | Nghiên cứu dữ liệu địa điểm Thành phố Hồ Chí Minh cho DynamoDB |
| Tháng 1 (Tháng 9/2025) | Xây dựng MVP backend với Bedrock LLM và DynamoDB            |
| Tháng 2 (Tháng 10/2025) | Triển khai bộ đệm, tích hợp giao diện người dùng            |
| Tháng 3 (Tháng 10/2025) | Ra mắt bản beta công khai, tối ưu hiệu suất, thu thập phản hồi |
| Sau Ra Mắt (Tháng 10/2025 trở đi) | Thêm các tính năng nâng cao (ví dụ: xếp hạng dựa trên ML, chế độ ngoại tuyến) |

---

## 6. Ước Tính Ngân Sách

### Biện Pháp Tối Ưu Chi Phí
- **Sử Dụng Miễn Phí**: Tận dụng các tầng miễn phí của AWS cho Lambda, DynamoDB, S3, CloudFront, Rekognition, và Cognito để giảm chi phí.
- **Bộ Đệm Tích Cực cho Bedrock**: Đạt tỷ lệ trúng bộ đệm 95% để giảm chi phí token AI từ $120 xuống dưới $15/tháng.
- **Xử Lý Theo Lô của Rekognition**: Kiểm tra ảnh không thực thời gian tiết kiệm ~$80 trong 8 tuần.
- **Kiểm Tra Tải Đơn Giản**: Kịch bản 100 người dùng × 10 phút thay vì 300 × 30 phút giảm chi phí tính toán.
- **Ghi Nhật Ký Giảm Bớt CloudWatch**: Chỉ ghi nhật ký lỗi tiết kiệm $50+ trong 8 tuần.
- **Không Sử Dụng Concurrency Được Cấp Phép**: Tránh chi phí Lambda nhàn rỗi.
- **Biến Môi Trường**: Sử dụng thay vì Secrets Manager để loại bỏ chi phí lưu trữ bí mật.
- **Chế Độ DynamoDB Theo Nhu Cầu**: Tất cả đọc/ghi miễn phí dưới tầng.
- **Tắt Origin Shield**: Tiết kiệm chi phí CloudFront.
- **Bộ Đệm Tài Sản Tĩnh**: Giảm chi phí truyền dữ liệu ra ngoài.

### Kịch Bản Ngân Sách Được Khuyến Nghị
Để đảm bảo nền tảng MapVibe hoạt động hiệu quả trong ngân sách AWS $200 cho chu kỳ phát triển và trình diễn 8 tuần ban đầu (hoàn thành vào ngày 22 tháng 10 năm 2025), chúng tôi khuyến nghị các kịch bản sau dựa trên các mức tối ưu hóa và sử dụng tài nguyên khác nhau:

- **Kịch Bản Tối Thiểu**: Tập trung vào các tính năng thiết yếu với sự phụ thuộc tối đa vào các tầng miễn phí. Bao gồm tắt các dịch vụ không quan trọng như WAF nếu không cần thiết, giới hạn các lần gọi Bedrock chỉ cho các truy vấn được bộ đệm (nhắm đến tỷ lệ trúng bộ đệm 98%+), và không thực hiện kiểm tra tải. Chi phí ước tính: <$50 trong 8 tuần. Phù hợp cho nguyên mẫu ban đầu nhưng có thể ảnh hưởng đến độ tin cậy của bản demo do các vấn đề khả năng mở rộng chưa được kiểm tra.

- **Kịch Bản Được Khuyến Nghị**: Cân bằng giữa chi phí và độ tin cậy bằng cách áp dụng tất cả các biện pháp tối ưu hóa chính nêu trên. Kịch bản này sử dụng bộ đệm tích cực (tỷ lệ trúng 95% cho Bedrock), xử lý theo lô cho Rekognition, kiểm tra tải đơn giản (100 người dùng × 10 phút), và chỉ ghi nhật ký lỗi trong CloudWatch. Nó đảm bảo độ trễ thấp và khả năng phục hồi trong khi vẫn nằm dưới ngân sách. Chi phí ước tính: ~$100-150 trong 8 tuần. Lý tưởng cho bản demo MVP ra mắt vào ngày 22 tháng 10 năm 2025, cung cấp trải nghiệm mạnh mẽ mà không có chi phí không cần thiết.

- **Kịch Bản Nâng Cao**: Bao gồm các điều khoản bổ sung cho việc sử dụng cao hơn sau khi ra mắt, như concurrency được cấp phép cho Lambda trong các thời điểm cao điểm và ghi nhật ký đầy đủ trong CloudWatch để gỡ lỗi chi tiết. Điều này tăng chi phí nhẹ nhưng nâng cao giám sát hiệu suất và kiểm tra khả năng mở rộng (ví dụ: 300 người dùng × 30 phút tải). Chi phí ước tính: ~$180-200 trong 8 tuần. Được khuyến nghị cho hoạt động tiếp tục sau ngày 22 tháng 10 năm 2025 nếu dự kiến có bản demo mở rộng hoặc lưu lượng truy cập cao hơn, vẫn nằm trong giới hạn ngân sách tổng thể.

Khuyến nghị: Kịch bản Được Khuyến Nghị đã được triển khai thành công cho ra mắt MVP vào ngày 22 tháng 10 năm 2025, đảm bảo độ tin cậy tối ưu, khả năng mở rộng, và kiểm soát chi phí trong ngân sách $200. Đối với hoạt động tiếp tục, hãy cân nhắc chuyển sang Kịch bản Nâng Cao khi cần thiết.

### Kiểm Soát & Giám Sát Chi Phí
- Tạo cảnh báo thanh toán và kích hoạt AWS Cost Explorer.
- Đánh dấu tất cả tài nguyên (Project=MapVibe, Environment=Dev).
- Kiểm tra hàng tuần:
  - Dữ liệu CloudFront > 50 GB/tuần
  - Tỷ lệ trúng bộ đệm Bedrock < 90%
  - Số lần gọi Lambda > 100K/tuần
  - Tổng chi phí > $15/tuần

---

## 7. Đánh Giá Rủi Ro

| Rủi Ro                          | Tác Động | Xác Suất | Giảm Thiểu                              |
|---------------------------------|----------|----------|----------------------------------------|
| Bất nhất dữ liệu DynamoDB        | Cao      | Trung bình | Xác thực và sao lưu dữ liệu định kỳ    |
| Phân tích sai LLM (VN/EN)       | Trung bình | Thấp    | Sử dụng mẫu lệnh định sẵn, xác thực    |
| Khả năng mở rộng dưới tải cao   | Trung bình | Trung bình | Tự động mở rộng không máy chủ, bộ đệm  |
| Vấn đề bảo mật (dữ liệu vị trí) | Cao      | Thấp     | Đồng ý rõ ràng từ người dùng, ẩn danh hóa truy vấn |

**Kế Hoạch Dự Phòng**: Sử dụng kết quả được bộ đệm từ DynamoDB hoặc tệp JSON cục bộ cho bản demo. Triển khai giới hạn tỷ lệ dựa trên IP cho người dùng chưa xác thực.

---

## 8. Kết Quả Mong Đợi

### Cải Tiến Kỹ Thuật
- **Tìm Kiếm Trò Chuyện**: Hỗ trợ ngôn ngữ tự nhiên cho tiếng Việt và tiếng Anh với độ trễ <10 giây, được hỗ trợ bởi Bedrock LLMs.
- **Tóm Tắt bởi AI**: Tóm tắt địa điểm do Bedrock tạo, làm mới sau 7 ngày hoặc sau 10 đánh giá mới.
- **Khả Năng Mở Rộng**: Kiến trúc không máy chủ với phân phối CDN toàn cầu qua CloudFront.
- **Kiểm Duyệt**: Rekognition đảm bảo nội dung do người dùng tạo (đánh giá, ảnh) an toàn.

### Giá Trị Dài Hạn
- **Cá Nhân Hóa**: Xếp hạng lại dựa trên ML và phân tích hành vi người dùng.
- **Hỗ Trợ Ngoại Tuyến**: PWA cho danh sách ngắn ngoại tuyến của các địa điểm.
- **Khả Năng Mở Rộng**: Tiềm năng tích hợp với các hệ thống đặt chỗ nội bộ.
- **Mở Rộng Ngữ Cảnh**: Đề xuất dựa trên thời tiết, sự kiện, hoặc xu hướng xã hội.

---

### Tài Liệu Tham Khảo / Đính Kèm
- **Liên Kết Máy Tính Giá AWS**: [Chưa Xác Định]
- **Sơ Đồ Kiến Trúc**: [Bản Nháp Chưa Hoàn Thành]
- **Kho GitHub**: [Chưa Thiết Lập]
- **Bảng Ngân Sách**: [Đang Tiến Hành]