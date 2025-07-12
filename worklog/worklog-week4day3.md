# 📘 Worklog - Ngày 04/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 04/06/2025
* **Thứ**: Thứ Tư
* **Tuần thực tập**: Tuần thứ 4/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 🔄 + Hứng thú khi xây hệ thống có tính sẵn sàng cao như sản phẩm thực tế

---

## 🎯 Mục tiêu ngày hôm nay

* Hoàn thành toàn bộ lab **"Xây dựng ứng dụng web có tính sẵn sàng cao"** theo lộ trình hướng dẫn
* Tạo kiến trúc 2-tier: Web + DB, sử dụng Auto Scaling Group và RDS Multi-AZ
* Cấu hình Application Load Balancer (ALB) và kiểm thử tính chịu lỗi
* Áp dụng các nguyên tắc High Availability (HA) và Fault Tolerance

---

## 💼 Công việc đã thực hiện

### 1. Chuẩn bị hạ tầng ứng dụng web nhiều lớp (2-tier) ⏱️ 2 giờ

* **Mô tả**: Thiết lập VPC với 2 AZ, tạo 2 subnet public cho web và private cho DB
* **Kết quả**: Có mô hình mạng chia lớp bảo mật rõ ràng
* **Tools/Tech**: VPC, Subnet, Route Table, IGW, NAT Gateway

---

### 2. Triển khai Web Server qua Auto Scaling Group ⏱️ 2 giờ

* **Mô tả**: Tạo Launch Template có script cài web server tự động, cấu hình ASG + ALB
* **Kết quả**: Hệ thống tự tạo EC2 khi cần, phân phối qua ALB
* **Tools/Tech**: EC2, ASG, ALB, Target Group, Security Group
* **User Data**:

  ```bash
  #!/bin/bash
  yum update -y
  yum install -y httpd
  systemctl start httpd
  echo "Welcome to FCJ High Availability App" > /var/www/html/index.html
  ```

---

### 3. Thiết lập cơ sở dữ liệu với Amazon RDS Multi-AZ ⏱️ 1.5 giờ

* **Mô tả**: Tạo database instance dạng Multi-AZ để đảm bảo hoạt động liên tục
* **Kết quả**: DB vẫn hoạt động khi AZ chính có sự cố
* **Tools/Tech**: Amazon RDS, MySQL, Subnet Group, Security Group

---

### 4. Kiểm thử tính sẵn sàng và fault tolerance ⏱️ 1.5 giờ

* **Mô tả**: Tắt 1 instance web để kiểm tra ASG scale in/out, thử failover DB
* **Kết quả**: ALB điều hướng lại hợp lý, DB failover mượt
* **Tools/Tech**: CloudWatch Alarm, ASG Termination Policy, RDS failover monitor

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Triển khai hệ thống Web Server có tính tự phục hồi (self-healing) thông qua ASG
* Cấu hình RDS Multi-AZ để đảm bảo tính liên tục dịch vụ cơ sở dữ liệu
* Hiểu và áp dụng kiến trúc phân lớp: Web Layer – Database Layer
* Tích hợp nhiều dịch vụ AWS để tạo hệ thống toàn diện

### 💡 Concepts & Theory

* **High Availability**: đảm bảo hệ thống vẫn hoạt động khi 1 thành phần bị lỗi
* **Auto Scaling**: duy trì số lượng EC2 theo nhu cầu
* **Load Balancer**: phân phối traffic, tăng khả năng mở rộng và chịu lỗi
* **Multi-AZ DB**: tránh downtime khi vùng có sự cố

### 🤝 Soft Skills

* Phối hợp nhiều dịch vụ phức tạp trong cùng một hệ thống
* Kiên nhẫn khi kiểm tra từng bước của hệ thống HA
* Ghi chú và mô tả rõ các thành phần trong sơ đồ kiến trúc

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: EC2 không cài được Apache từ user-data

* **Mô tả**: EC2 launch nhưng không truy cập được trang web
* **Impact**: ALB trả về lỗi 502
* **Root Cause**: Script cài đặt trong user-data bị lỗi quyền
* **Solution**: Thêm `#!/bin/bash` và `yum update -y` trước khi cài httpd
* **Result**: EC2 chạy đúng, trang web hiển thị bình thường
* **Lesson**: Phải kiểm tra kỹ User Data script trước khi dùng cho Launch Template

---

### Vấn đề 2: RDS không thể tạo Multi-AZ

* **Mô tả**: Khi chọn Multi-AZ thì báo lỗi subnet
* **Impact**: Không triển khai được DB HA
* **Root Cause**: Thiếu RDS Subnet Group trải đều trên nhiều AZ
* **Solution**: Tạo subnet group gồm ít nhất 2 private subnet ở các AZ khác nhau
* **Result**: Tạo thành công DB instance Multi-AZ
* **Lesson**: AWS yêu cầu rõ về vùng AZ và subnet cho các dịch vụ HA

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Hoàn thành toàn bộ lab xây dựng hệ thống web sẵn sàng cao
* Học được cách tích hợp ASG, ALB, RDS thành kiến trúc hoàn chỉnh
* Kiểm thử thành công khả năng tự động phục hồi và duy trì hoạt động

### 🔄 What could be improved?

* Cần thêm một bước để log lại tất cả log ứng dụng vào CloudWatch
* Có thể dùng CloudFormation để tự động hóa toàn bộ setup
* Thử nghiệm failover trong điều kiện thật như ngắt AZ

### 💡 Key Insights

* Hệ thống HA không chỉ là “chạy nhiều máy” mà là kiến trúc đồng bộ tự điều chỉnh
* ASG + ALB + RDS Multi-AZ là bộ công cụ cực mạnh giúp đảm bảo uptime cao
* Tư duy High Availability cần thiết cho mọi hệ thống production trên cloud

Worklog created by: Tran Quang Trong
Next review: 05/05/2025