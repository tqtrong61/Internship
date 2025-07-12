# 📘 Worklog - Ngày 05/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 04/06/2025
* **Thứ**: Thứ Năm
* **Tuần thực tập**: Tuần thứ 4/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 🌍 + Hào hứng khi khám phá điện toán phân tán toàn cầu với Lambda\@Edge

---

## 🎯 Mục tiêu ngày hôm nay

* Hiểu kiến trúc và use-case của **Lambda\@Edge** trong hệ thống phân phối nội dung
* Thực hành cấu hình **Amazon CloudFront** với S3 origin
* Viết một hàm Lambda\@Edge đơn giản để **chuyển hướng ngôn ngữ (i18n)** hoặc chỉnh sửa header
* Deploy và kiểm tra các request được xử lý tại edge location

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu tổng quan Lambda\@Edge và CDN CloudFront ⏱️ 1.5 giờ

* **Mô tả**: Đọc tài liệu chính thức và bài học từ [AWS Study Group](https://000130.awsstudygroup.com/vi/)
* **Kết quả**: Nắm được cơ chế hoạt động của Lambda\@Edge và khi nào nên dùng
* **Tools/Tech**: CloudFront, Lambda\@Edge, IAM Role

---

### 2. Tạo CloudFront Distribution với S3 Static Website ⏱️ 1.5 giờ

* **Mô tả**: Dùng S3 chứa website tĩnh, phân phối qua CloudFront
* **Kết quả**: Website chạy nhanh hơn và được truy cập qua các edge location
* **Lưu ý**: Phải bật Static Website Hosting và set đúng index.html
* **Command CLI** (nếu cần):

  ```bash
  aws cloudfront create-distribution \
    --origin-domain-name fcj-static-site.s3.amazonaws.com
  ```

---

### 3. Viết và deploy Lambda\@Edge để chuyển hướng locale tự động ⏱️ 2.5 giờ

* **Mô tả**: Tạo function chuyển hướng dựa theo header `Accept-Language`
* **Kết quả**: Trình duyệt tiếng Việt sẽ được redirect về `/vi/index.html`, còn English → `/en/index.html`
* **Tools/Tech**: Lambda (us-east-1), CloudFront Association
* **Code mẫu**:

  ```js
  exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;
    const headers = request.headers;
    const lang = headers['accept-language']?.[0]?.value || 'en';

    if (lang.startsWith('vi')) {
      request.uri = '/vi/index.html';
    } else {
      request.uri = '/en/index.html';
    }

    callback(null, request);
  };
  ```

---

### 4. Gắn function Lambda vào sự kiện Viewer Request của CloudFront ⏱️ 1 giờ

* **Mô tả**: Deploy function lên us-east-1 và attach vào Viewer Request
* **Kết quả**: Lambda được thực thi tại edge location, kiểm tra bằng CloudFront logs
* **Công cụ**: AWS Console, IAM Role `lambda-execute`, Lambda Replication

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Cấu hình CloudFront Distribution cho S3 website
* Viết Lambda function xử lý edge events (Viewer Request)
* Deploy Lambda\@Edge ở đúng vùng `us-east-1` và liên kết với CloudFront
* Kiểm thử chuyển hướng ngôn ngữ theo tiêu đề HTTP

### 💡 Concepts & Theory

* **Lambda\@Edge**: mở rộng khả năng xử lý serverless đến edge của CloudFront
* **Viewer Request**: sự kiện diễn ra trước khi người dùng truy cập nội dung
* **Edge Location**: trung tâm dữ liệu gần nhất với người dùng, tăng tốc độ phản hồi
* **Stateless Function**: hàm Lambda\@Edge không giữ trạng thái, nên logic phải nhẹ

### 🤝 Soft Skills

* Quản lý phiên bản Lambda và lifecycle trên nhiều region
* Đọc hiểu và debug logs từ CloudFront và Lambda với `console.log`
* Kiên nhẫn chờ propagation và học cách theo dõi các edge deployment

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Lambda\@Edge không chạy dù đã liên kết

* **Mô tả**: Dù đã deploy nhưng website không redirect
* **Impact**: Người dùng không thấy nội dung theo ngôn ngữ
* **Root Cause**: Chưa publish version của Lambda khi attach vào CloudFront
* **Solution**: Dùng "Publish new version" trước khi attach → Viewer Request
* **Result**: Lambda được gọi đúng
* **Lesson**: Lambda\@Edge **chỉ hoạt động với phiên bản đã publish**

---

### Vấn đề 2: Không thể deploy Lambda\@Edge ngoài `us-east-1`

* **Mô tả**: Tạo function ở `ap-southeast-1` thì không dùng được với CloudFront
* **Impact**: Không thể attach function
* **Root Cause**: Lambda\@Edge **chỉ hỗ trợ triển khai từ vùng US East (N. Virginia)**
* **Solution**: Deploy lại ở `us-east-1`, sau đó attach
* **Result**: Function replicate ra các edge location
* **Lesson**: Lambda\@Edge có quy tắc vùng rất nghiêm ngặt

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Thành công trong việc gắn Lambda\@Edge với CloudFront
* Tự viết logic chuyển hướng dựa vào Accept-Language
* Thấy được rõ ràng lợi ích của edge computing: nhanh, giảm tải backend

### 🔄 What could be improved?

* Nên dùng test A/B với CloudFront behavior rules
* Thử nghiệm thêm sự kiện khác như Origin Request, Viewer Response
* Bổ sung thêm unit test hoặc monitoring cho Lambda function

### 💡 Key Insights

* Lambda\@Edge mang lại khả năng xử lý thông minh ngay tại nơi gần người dùng nhất
* Đây là giải pháp lý tưởng cho **personalization**, **localization**, và **security header injection**
* Điện toán biên là tương lai của các hệ thống phân phối nội dung hiện đại

Worklog created by: Tran Quang Trong