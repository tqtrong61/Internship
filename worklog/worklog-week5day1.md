# 📘 Worklog - Ngày 09/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 09/06/2025
* **Thứ**: Thứ Hai
* **Tuần thực tập**: Tuần thứ 5/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: ⚙️ + Hào hứng khi viết được function tự động chạy mà không cần máy chủ

---

## 🎯 Mục tiêu ngày hôm nay

* Làm quen với khái niệm **Serverless** và kiến trúc Lambda trên AWS
* Tạo hàm Lambda đầu tiên để tự động hoá quy trình xử lý file trên S3
* Kết nối Lambda với trigger từ S3 và CloudWatch Events
* Tìm hiểu cấu trúc function, permission, và logging trong CloudWatch

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu tổng quan AWS Lambda và Serverless Architecture ⏱️ 1.5 giờ

* **Mô tả**: Nắm khái niệm "No server to manage" và trường hợp sử dụng thực tế
* **Kết quả**: Hiểu lifecycle của một Lambda function và lợi ích về chi phí, hiệu suất
* **Tài liệu tham khảo**: [AWS Study Group - Lambda](https://000022.awsstudygroup.com/vi/)

---

### 2. Tạo Lambda Function đầu tiên xử lý file S3 ⏱️ 2 giờ

* **Mô tả**: Viết function xử lý khi người dùng upload file `.txt` lên S3 bucket
* **Kết quả**: Tự động đọc nội dung file và ghi log vào CloudWatch
* **Code mẫu (Node.js)**:

  ```js
  const AWS = require('aws-sdk');
  const s3 = new AWS.S3();

  exports.handler = async (event) => {
    const bucket = event.Records[0].s3.bucket.name;
    const key = event.Records[0].s3.object.key;
    
    const data = await s3.getObject({ Bucket: bucket, Key: key }).promise();
    const content = data.Body.toString('utf-8');
    
    console.log("📄 File content:", content);
    return { statusCode: 200, body: "Processed" };
  };
  ```

---

### 3. Tạo Trigger từ S3 để gọi Lambda ⏱️ 1 giờ

* **Mô tả**: Cấu hình sự kiện “ObjectCreated” trên S3 để gọi Lambda
* **Kết quả**: Mỗi khi upload file mới, Lambda tự chạy và log nội dung
* **Tools/Tech**: S3 Event Notification, Lambda Permission, IAM Role

---

### 4. Tự động hoá định kỳ với CloudWatch Event Rule ⏱️ 1.5 giờ

* **Mô tả**: Tạo lịch chạy Lambda mỗi 5 phút để kiểm tra trạng thái dịch vụ
* **Kết quả**: Lambda tự động thực hiện công việc mà không cần user tương tác
* **Tools/Tech**: CloudWatch Scheduler, EventBridge Rule, Lambda Trigger

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Tạo, deploy và test Lambda Function trên AWS Console
* Kết nối Lambda với các nguồn trigger như S3, EventBridge
* Sử dụng AWS SDK để tương tác với tài nguyên khác trong Lambda
* Quản lý IAM Role, policy để Lambda có quyền cần thiết

### 💡 Concepts & Theory

* **Serverless** = không cần quản lý hạ tầng, chỉ tập trung vào logic xử lý
* Lambda có thể chạy theo sự kiện từ S3, DynamoDB, API Gateway, EventBridge...
* Lambda chịu trách nhiệm nhỏ, thực hiện nhanh, tốn chi phí thấp

### 🤝 Soft Skills

* Debug qua log trong CloudWatch Logs
* Lập kế hoạch logic xử lý event-driven
* Ghi chú quy trình tự động hóa rõ ràng để dễ mở rộng

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Lambda không đọc được file S3

* **Mô tả**: Lambda function báo lỗi AccessDenied
* **Impact**: Không thể xử lý file sau khi upload
* **Root Cause**: IAM Role của Lambda chưa có quyền `s3:GetObject`
* **Solution**: Thêm quyền `s3:GetObject` vào policy của Lambda execution role
* **Result**: Đọc file thành công
* **Lesson**: Với Lambda, **IAM là chìa khoá để mọi thứ hoạt động đúng**

---

### Vấn đề 2: Function timeout khi chạy tác vụ lớn

* **Mô tả**: Hàm xử lý file lớn thì bị timeout
* **Impact**: Lambda dừng giữa chừng
* **Root Cause**: Thời gian mặc định 3 giây không đủ
* **Solution**: Tăng timeout lên 30 giây trong phần cấu hình Lambda
* **Result**: Chạy hoàn chỉnh
* **Lesson**: Tùy workload mà cần tối ưu **timeout và memory size** hợp lý

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Tự tạo được Lambda function đơn giản xử lý S3 upload
* Hiểu cách dùng EventBridge để lập lịch job tự động
* Nắm rõ kiến trúc Serverless và luồng xử lý không cần máy chủ

### 🔄 What could be improved?

* Nên thử viết function ở ngôn ngữ khác như Python
* Tích hợp với API Gateway để biến Lambda thành backend cho web
* Dùng SAM hoặc CDK để quản lý infrastructure as code

### 💡 Key Insights

* AWS Lambda là trái tim của kiến trúc serverless, mở ra nhiều khả năng tự động hóa
* Việc gắn Lambda với sự kiện giúp xây dựng ứng dụng phản ứng nhanh và tiết kiệm chi phí
* Làm chủ IAM và cấu trúc sự kiện là yếu tố then chốt khi làm việc với Lambda

Worklog created by: Tran Quang Trong
Next review: 10/06/2025