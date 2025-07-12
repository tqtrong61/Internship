# 📘 Worklog - Ngày 19/05/2025

## 📅 Thông tin cơ bản

- **Ngày**: 19/05/2025  
- **Thứ**: Thứ Năm  
- **Tuần thực tập**: Tuần thứ 2/12  
- **Thời gian làm việc**: 9:00 - 17:00  
- **Mood**: 🌍 + Hứng thú khi lần đầu host website lên cloud

---

## 🎯 Mục tiêu ngày hôm nay

- Tìm hiểu cách Amazon S3 hỗ trợ lưu trữ và phân phối nội dung tĩnh
- Tạo bucket S3 và upload website HTML tĩnh
- Cấu hình quyền public access và bật tính năng Static Website Hosting
- Kiểm tra website hoạt động từ trình duyệt

---

## 💼 Công việc đã thực hiện

### 1. Học tổng quan về Static Website trên S3 ⏱️ 1.5 giờ

- **Mô tả**: Đọc hướng dẫn trên AWS Study Group và tài liệu chính thức
- **Kết quả**: Nắm được cách S3 phục vụ website tĩnh mà không cần EC2
- **Tools/Tech**: Amazon S3, HTML/CSS
- **Links**: [Hosting S3 Website](https://000057.awsstudygroup.com/vi/)

---

### 2. Tạo bucket và upload source website ⏱️ 2 giờ

- **Mô tả**: Tạo S3 bucket với tên duy nhất toàn cầu, upload file index.html và style.css
- **Kết quả**: File được lưu trữ đúng cấu trúc, set quyền Public Read
- **Tools/Tech**: AWS S3 Console, HTML Editor
- **Links**: [S3 Bucket Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)

---

### 3. Bật Static Website Hosting và truy cập website ⏱️ 2 giờ

- **Mô tả**: Cấu hình endpoint, set index.html làm trang mặc định
- **Kết quả**: Truy cập được website từ đường dẫn public S3
- **Tools/Tech**: S3, Website Hosting Settings
- **Links**: [Enable Static Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteEndpoints.html)

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

- Tạo S3 bucket đúng chuẩn DNS naming
- Cấu hình public access và policy S3 bucket
- Bật tính năng Static Website Hosting và kiểm tra endpoint

### 💡 Concepts & Theory

- S3 có thể dùng như web server cho nội dung tĩnh (HTML, CSS, JS)
- Website tĩnh không hỗ trợ xử lý phía server (PHP, database…)
- Static Hosting không cần EC2 hay backend, tiết kiệm chi phí

### 🤝 Soft Skills

- Kỹ năng viết và chỉnh sửa HTML đơn giản
- Cẩn thận khi thiết lập quyền truy cập công khai (public access)
- Ghi chú và tổ chức file HTML/CSS rõ ràng trước khi upload

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Website báo lỗi "Access Denied"

- **Mô tả**: Khi truy cập endpoint, không hiển thị trang web
- **Impact**: Website không hoạt động
- **Root Cause**: File không có quyền public hoặc policy bucket bị sai
- **Solution**: Bật “Block all public access” = OFF, cập nhật bucket policy cho phép GetObject
- **Result**: Website hiển thị bình thường
- **Lesson**: Cấu hình quyền là bước bắt buộc để website hoạt động

---

### Vấn đề 2: Không tìm thấy file index.html

- **Mô tả**: S3 báo lỗi khi không thấy file chính
- **Impact**: Người dùng truy cập sẽ gặp lỗi
- **Root Cause**: Chưa set index.html trong phần static hosting
- **Solution**: Truy cập “Properties” > “Static Website Hosting” và điền đúng file
- **Result**: Website hoạt động ổn định
- **Lesson**: Cần kiểm tra cấu hình cả ở cấp bucket lẫn file

---

## 💭 Reflection & Insights

### ✅ What went well today?

- Website tĩnh đầu tiên được host thành công trên cloud
- Nắm rõ luồng triển khai từ local lên cloud qua S3
- Hiểu rõ các thiết lập quyền và cấu trúc thư mục cần thiết

### 🔄 What could be improved?

- Nên tích hợp thêm CloudFront để tăng tốc tải trang
- Dùng Route 53 để gắn domain tùy chỉnh
- Tìm hiểu thêm versioning để quản lý file thay đổi

### 💡 Key Insights

- Hosting website tĩnh bằng S3 cực kỳ đơn giản và tiết kiệm
- Cấu hình quyền truy cập là điểm dễ sai với người mới
- Rất phù hợp cho landing page, portfolio hoặc tài liệu nội bộ

Worklog created by: Tran Quang Trong
Next review: 20/05/2025