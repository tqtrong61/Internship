# 📘 Worklog - Ngày 23/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 23/06/2025
* **Thứ**: Thứ Hai
* **Tuần thực tập**: Tuần thứ 7/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 🔐 + Ấn tượng vì có thể SSH vào EC2 mà không cần mở port 22

---

## 🎯 Mục tiêu ngày hôm nay

* Làm quen với **AWS Systems Manager (SSM)** và tính năng **Session Manager**
* Cấu hình truy cập từ xa vào EC2 **không cần SSH key, không cần mở port**
* Quản lý phân quyền truy cập EC2 thông qua **IAM**
* Trải nghiệm truy cập EC2 bằng **AWS Console, AWS CLI và CloudShell**

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu Session Manager và tính năng bảo mật ⏱️ 1 giờ

* **Mô tả**: Đọc tài liệu và hiểu các lợi ích: bảo mật cao, không cần SSH, audit đầy đủ
* **Kết quả**: Nắm rõ kiến trúc hoạt động: EC2 → Agent → SSM → IAM → Console

---

### 2. Cài đặt & cấu hình SSM Agent trên EC2 ⏱️ 1 giờ

* **Mô tả**: Đảm bảo EC2 có cài **SSM Agent** và gán **IAM Role** phù hợp
* **Kết quả**: EC2 đã đăng ký thành công với Systems Manager
* **IAM Role yêu cầu**: `AmazonSSMManagedInstanceCore`

---

### 3. Truy cập EC2 từ Console và kiểm tra ⏱️ 1.5 giờ

* **Mô tả**: Sử dụng “Start Session” từ giao diện SSM để đăng nhập vào EC2
* **Kết quả**: Giao diện terminal web-based hoạt động mượt, không cần mở SSH
* **Ghi chú**: Cổng 22 hoàn toàn không cần mở → bảo mật cao hơn rất nhiều

---

### 4. Thử truy cập từ AWS CLI và CloudShell ⏱️ 1.5 giờ

* **Mô tả**: Cài đặt AWS CLI và sử dụng lệnh `start-session` để kết nối
* **Kết quả**: Truy cập thành công vào EC2 ngay từ command line
* **Lệnh sử dụng**:

  ```bash
  aws ssm start-session --target <Instance-ID>
  ```

---

### 5. Cấu hình logging và kiểm soát quyền truy cập ⏱️ 2 giờ

* **Mô tả**: Bật log lưu session vào CloudWatch Logs và S3
* **Kết quả**: Mỗi session đều có log chi tiết để kiểm tra và audit
* **Ghi chú**: Có thể dùng CloudTrail để theo dõi ai truy cập, khi nào và làm gì

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Thiết lập IAM Role và Policy cho EC2 sử dụng SSM
* Cài và cấu hình Systems Manager Agent
* Truy cập EC2 từ xa không cần key pair
* Ghi log phiên làm việc với SSM vào CloudWatch

### 💡 Concepts & Theory

* **Session Manager** là giải pháp truy cập EC2 an toàn, không phụ thuộc SSH
* Giảm thiểu tấn công bằng cách **không mở cổng 22**
* **IAM + CloudTrail** cung cấp kiểm soát và theo dõi toàn diện truy cập
* Hệ thống phù hợp với chuẩn bảo mật (PCI, HIPAA…)

### 🤝 Soft Skills

* Biết cách tổ chức bài test theo checklist kỹ thuật + bảo mật
* Cẩn thận khi gán IAM Role để tránh lỗ hổng
* Đọc và phân tích log truy cập để phát hiện bất thường

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: EC2 không hiển thị trong Systems Manager

* **Mô tả**: EC2 không hiển thị trong mục “Managed Instances”
* **Impact**: Không thể kết nối qua Session Manager
* **Root Cause**: Thiếu IAM Role hoặc SSM Agent chưa chạy
* **Solution**:

  * Gán IAM Role với policy `AmazonSSMManagedInstanceCore`
  * Cài đặt lại SSM Agent nếu chưa có
* **Result**: EC2 hiển thị và kết nối được
* **Lesson**: Muốn truy cập qua SSM, **IAM và agent phải cấu hình chuẩn xác**

---

### Vấn đề 2: Không ghi được log phiên session

* **Mô tả**: Log không lưu về CloudWatch như mong đợi
* **Impact**: Mất khả năng audit
* **Root Cause**: Chưa tạo hoặc gán đúng CloudWatch log group & policy
* **Solution**: Tạo log group, gán quyền ghi logs cho IAM Role
* **Result**: Logs được ghi đầy đủ mỗi session
* **Lesson**: **Logging = Visibility** → quan trọng trong môi trường production

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Lần đầu truy cập EC2 mà không cần SSH, thấy đơn giản và bảo mật hơn
* Ghi log và kiểm soát truy cập rất dễ dàng, không cần cài đặt thêm công cụ
* Rất phù hợp cho môi trường doanh nghiệp cần bảo mật cao

### 🔄 What could be improved?

* Nên kết hợp thêm CloudTrail để theo dõi mọi hoạt động session
* Viết automation script gán Role + bật logging + test kết nối
* Học cách dùng Systems Manager để chạy lệnh hàng loạt trên nhiều EC2 (Run Command)

### 💡 Key Insights

* Session Manager không chỉ thay thế SSH, mà còn mở rộng khả năng audit, automation, bảo mật
* Việc bỏ SSH giúp giảm thiểu tấn công và lỗi do rò rỉ private key
* Với IAM, có thể giới hạn rất chi tiết: ai được truy cập, truy cập EC2 nào, trong khung giờ nào

Worklog created by: Tran Quang Trong