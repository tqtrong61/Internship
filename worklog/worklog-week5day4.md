# 📘 Worklog - Ngày 12/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 12/06/2025
* **Thứ**: Thứ Năm
* **Tuần thực tập**: Tuần thứ 5/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: ⚙️ + Thích thú khi không cần lo quản lý máy chủ

---

## 🎯 Mục tiêu ngày hôm nay

* Hiểu cơ bản về kiến trúc **Serverless** và vai trò của AWS Lambda
* Viết Lambda function để tự động xử lý sự kiện trong AWS
* Kết nối Lambda với trigger từ S3 và CloudWatch Events
* Khám phá việc tích hợp Lambda trong các workflow tự động hóa

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu khái niệm Serverless và Lambda ⏱️ 1.5 giờ

* **Mô tả**: Đọc tài liệu về Lambda function, cách hoạt động và use-case phổ biến
* **Kết quả**: Hiểu được Serverless = không cần quản lý hạ tầng, tự động scale
* **Nguồn tham khảo**: [AWS Study Group - Lambda](https://000022.awsstudygroup.com/vi/)

---

### 2. Viết Lambda function xử lý file tải lên S3 ⏱️ 2 giờ

* **Mô tả**: Lambda đọc file `.txt` người dùng upload lên bucket và log nội dung
* **Kết quả**: Tự động ghi log vào CloudWatch mỗi khi có file mới
* **Tools/Tech**: AWS Lambda, S3, IAM Role, Node.js
* **Code mẫu**:

  ```js
  const AWS = require('aws-sdk');
  const s3 = new AWS.S3();

  exports.handler = async (event) => {
    const bucket = event.Records[0].s3.bucket.name;
    const key = event.Records[0].s3.object.key;
    const data = await s3.getObject({ Bucket: bucket, Key: key }).promise();
    const content = data.Body.toString('utf-8');
    console.log("Nội dung file:", content);
    return { statusCode: 200, body: "OK" };
  };
  ```

---

### 3. Tạo trigger từ S3 và test sự kiện thực tế ⏱️ 1 giờ

* **Mô tả**: Cấu hình sự kiện “ObjectCreated” trong bucket để gọi Lambda
* **Kết quả**: Khi người dùng upload file, Lambda tự động xử lý
* **Lưu ý**: Cần cấu hình đúng quyền `s3:GetObject` cho Lambda Role

---

### 4. Tự động hoá định kỳ bằng CloudWatch Events (EventBridge) ⏱️ 1.5 giờ

* **Mô tả**: Lambda tự chạy mỗi 10 phút để gửi thông báo kiểm tra tình trạng hệ thống
* **Kết quả**: Tạo job định kỳ mà không cần cron hoặc EC2
* **Tools/Tech**: EventBridge Rule, Lambda, CloudWatch Logs

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Viết và triển khai Lambda function đơn giản bằng Node.js
* Trigger Lambda từ các sự kiện AWS như S3 upload và CloudWatch timer
* Quản lý IAM Role cho phép Lambda truy cập dịch vụ khác (S3, logs)
* Debug qua CloudWatch Logs

### 💡 Concepts & Theory

* Serverless = “code chạy mà không lo hạ tầng”, thích hợp cho event-driven
* Lambda có thể gọi bởi: API Gateway, S3, EventBridge, DynamoDB...
* Execution model của Lambda = ngắn gọn, không trạng thái (stateless), scale tự động
* Tự động hóa giúp tiết kiệm chi phí và tăng độ ổn định

### 🤝 Soft Skills

* Kỹ năng cấu hình IAM đúng để tránh lỗi AccessDenied
* Ghi chép đầy đủ quy trình thiết lập để có thể tái sử dụng
* Rèn tính kiên nhẫn khi deploy và test các event khó debug

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Lambda không có quyền truy cập file S3

* **Mô tả**: Log báo lỗi `AccessDenied` khi gọi `s3.getObject`
* **Impact**: Lambda fail, không xử lý được file
* **Root Cause**: Thiếu quyền `s3:GetObject` trong IAM role
* **Solution**: Thêm policy `AmazonS3ReadOnlyAccess` hoặc `s3:GetObject` thủ công
* **Result**: Lambda chạy mượt
* **Lesson**: Lambda rất nhạy với quyền — cần hiểu rõ IAM policy

---

### Vấn đề 2: CloudWatch Event không trigger Lambda

* **Mô tả**: Đặt lịch rồi nhưng Lambda không tự chạy
* **Impact**: Không có automation định kỳ
* **Root Cause**: EventBridge rule chưa target đúng ARN của Lambda
* **Solution**: Gắn lại rule đúng ARN và check log CloudWatch
* **Result**: Chạy đúng định kỳ
* **Lesson**: Với automation, cần kiểm tra từng chi tiết từ rule → function

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Đã triển khai workflow serverless đơn giản với Lambda + S3 + EventBridge
* Biết cách cấu hình và gán trigger cho Lambda
* Thấy rõ tiềm năng tự động hóa bằng Serverless

### 🔄 What could be improved?

* Nên thử viết thêm hàm xử lý phức tạp như resize ảnh, gửi mail...
* Học cách dùng AWS SAM hoặc CDK để quản lý hạ tầng Lambda
* Ghi log đầy đủ để dễ trace về sau

### 💡 Key Insights

* Lambda rất mạnh trong các workflow automation nhỏ, linh hoạt
* Sự kiện S3 → Lambda là mẫu thiết kế phổ biến trong xử lý file
* Tự động hóa bằng serverless giúp tiết kiệm chi phí, thời gian và tài nguyên rất lớn

Worklog created by: Tran Quang Trong