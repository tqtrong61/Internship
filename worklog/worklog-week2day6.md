# 📘 Worklog - Ngày 24/05/2025

## 📅 Thông tin cơ bản

* **Ngày**: 24/05/2025
* **Thứ**: Thứ Bảy
* **Tuần thực tập**: Tuần thứ 2/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 🚀 + Hứng thú với khả năng tự động mở rộng của hệ thống trên AWS

---

## 🎯 Mục tiêu ngày hôm nay

* Tìm hiểu cách hoạt động của **Auto Scaling Group (ASG)** trong việc duy trì hiệu năng và tính sẵn sàng
* Tạo Launch Template và cấu hình ASG cho ứng dụng “FCJ Management”
* Thiết lập Scaling Policies dựa trên CPU Utilization
* Kiểm thử khả năng scale in/out khi hệ thống thay đổi tải

---

## 💼 Công việc đã thực hiện

### 1. Hiểu mô hình Auto Scaling Group trên AWS ⏱️ 1.5 giờ

* **Mô tả**: Đọc tài liệu về Launch Template, ASG, Scaling Policies
* **Kết quả**: Hiểu được vai trò của EC2 Launch Template, Load Balancer, và Metrics trong kiến trúc autoscale
* **Tools/Tech**: AWS EC2, ASG, CloudWatch
* **Links**: [Auto Scaling Guide](https://000006.awsstudygroup.com/vi/)

---

### 2. Tạo Launch Template và Auto Scaling Group ⏱️ 2.5 giờ

* **Mô tả**: Tạo Launch Template sử dụng AMI của FCJ Management App đã cài sẵn, cấu hình Auto Scaling Group với min=1, max=3, desired=1
* **Kết quả**: Có thể scale EC2 instance khi cần thiết, ứng dụng luôn sẵn sàng
* **Tools/Tech**: Launch Template, EC2, ASG, Security Groups

---

### 3. Thiết lập Scaling Policy và kiểm thử ⏱️ 2 giờ

* **Mô tả**: Cấu hình scaling dựa trên CPU > 50%, test bằng cách stress CPU trong instance
* **Kết quả**: Auto Scaling Group tự động tạo thêm EC2 khi cần, và giảm khi tải thấp
* **Tools/Tech**: CloudWatch Metrics, ASG Policy, stress-ng tool

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Tạo Launch Template chuẩn bị sẵn ứng dụng để auto deploy
* Cấu hình Auto Scaling Group có gắn với Application Load Balancer
* Thiết lập Scaling Policy theo metric CPU hoặc số lượng request

### 💡 Concepts & Theory

* Auto Scaling giúp hệ thống **mở rộng khi tải cao** và **thu hẹp khi tải thấp**
* Launch Template là bước nền định nghĩa máy chủ mẫu cho ASG sử dụng
* Scaling Policies có thể dựa theo nhiều metric như CPU, Request Count, hoặc Schedule

### 🤝 Soft Skills

* Hiểu logic vận hành của hệ thống khi được phân phối động
* Biết cách sử dụng CloudWatch để quan sát trạng thái hạ tầng
* Kỹ năng thử nghiệm tải (load testing) để kiểm tra behavior hệ thống

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Auto Scaling không khởi tạo EC2 như mong đợi

* **Mô tả**: Tải tăng cao nhưng hệ thống không scale out
* **Impact**: Ứng dụng chậm, không đủ instance để phục vụ
* **Root Cause**: Scaling Policy chưa được gắn hoặc thiếu quyền IAM
* **Solution**: Gắn lại đúng IAM Role và kiểm tra liên kết Policy
* **Result**: Scaling hoạt động đúng như kỳ vọng
* **Lesson**: Cần xác minh tất cả bước cấu hình, nhất là quyền IAM

---

### Vấn đề 2: Instance mới tạo không hoạt động ứng dụng FCJ

* **Mô tả**: EC2 tạo ra nhưng không load được ứng dụng web
* **Impact**: FCJ App không phục vụ người dùng
* **Root Cause**: Launch Template không tích hợp sẵn User Data để auto deploy app
* **Solution**: Thêm script vào User Data để EC2 tự động cài đặt app khi launch
* **Result**: Mỗi instance mới đều hoạt động app như mong đợi
* **Lesson**: Launch Template cần cấu hình đủ để EC2 "tự sống" khi tạo

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Tạo được hệ thống FCJ Management có khả năng scale động
* Hiểu được cách hoạt động từ Launch Template đến Load Balancer và Scaling
* Quan sát và phân tích CloudWatch Metrics để kích hoạt scale

### 🔄 What could be improved?

* Nên tích hợp thêm Alert khi scaling xảy ra
* Có thể áp dụng Target Tracking hoặc Predictive Scaling thay vì chỉ metric-based
* Thử gắn thêm Route53 để phân phối theo vùng địa lý

### 💡 Key Insights

* Auto Scaling giúp giảm chi phí và tăng hiệu suất mà không cần giám sát thủ công
* Launch Template và User Data là nền tảng cho tính “tự triển khai”
* Đây là bước chuẩn bị cho hệ thống lớn mạnh hơn theo thời gian và lượng người dùng

Worklog created by: Tran Quang Trong
Next review: 25/05/2025
