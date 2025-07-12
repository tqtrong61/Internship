# 📘 Worklog - Ngày 25/05/2025

## 📅 Thông tin cơ bản

* **Ngày**: 25/05/2025
* **Thứ**: Chủ Nhật
* **Tuần thực tập**: Tuần thứ 2/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 📈 + Tập trung phân tích dữ liệu hệ thống và cảnh báo chủ động

---

## 🎯 Mục tiêu ngày hôm nay

* Làm quen với Amazon CloudWatch và các thành phần chính như Logs, Metrics, Alarms, Dashboards
* Cấu hình giám sát hiệu suất EC2: CPU, Disk, Memory
* Tạo cảnh báo tự động khi vượt ngưỡng CPU
* Thử gửi log ứng dụng từ EC2 lên CloudWatch Logs

---

## 💼 Công việc đã thực hiện

### 1. Hiểu kiến trúc và chức năng chính của CloudWatch ⏱️ 1.5 giờ

* **Mô tả**: Đọc hướng dẫn AWS Study Group và tài liệu chính thức
* **Kết quả**: Nắm được vai trò của CloudWatch Logs, Metrics, Alarms, Dashboards
* **Tools/Tech**: Amazon CloudWatch Console
* **Links**: [Giám sát với CloudWatch](https://000008.awsstudygroup.com/vi/)

---

### 2. Theo dõi hiệu suất EC2 instance với Metrics ⏱️ 2 giờ

* **Mô tả**: Quan sát metric CPUUtilization theo thời gian thực, set filter thời gian
* **Kết quả**: Biết cách đọc biểu đồ và phân tích mức độ sử dụng tài nguyên
* **Tools/Tech**: EC2, CloudWatch Metrics, Graph view

---

### 3. Tạo cảnh báo khi CPU vượt ngưỡng ⏱️ 1.5 giờ

* **Mô tả**: Tạo Alarm khi CPU > 70% trong 5 phút, gửi email qua SNS
* **Kết quả**: Nhận email cảnh báo ngay khi test CPU load bằng `stress-ng`
* **Tools/Tech**: CloudWatch Alarm, SNS, Email
* **Links**: [Create Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)

---

### 4. Gửi log ứng dụng từ EC2 lên CloudWatch Logs ⏱️ 2 giờ

* **Mô tả**: Cài đặt agent CloudWatch Logs lên EC2 Ubuntu, cấu hình để gửi file `/var/log/syslog`
* **Kết quả**: Log được ghi nhận trên CloudWatch Logs trong Log Group riêng
* **Tools/Tech**: CloudWatch Logs Agent, IAM Role, EC2
* **Links**: [CloudWatch Agent Setup](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/QuickStartEC2Instance.html)

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Quan sát và đọc biểu đồ metrics trên CloudWatch
* Tạo Alarm kết nối với SNS để cảnh báo real-time
* Cài đặt CloudWatch Agent để đẩy log từ EC2
* Phân tích log trên giao diện Logs Insights

### 💡 Concepts & Theory

* **Metrics**: là dữ liệu định lượng như CPU, Disk, Network
* **Logs**: là dữ liệu văn bản từ ứng dụng, hệ điều hành, v.v.
* **Alarms**: hoạt động khi metric vượt ngưỡng và kích hoạt hành động
* **Dashboards**: giúp trực quan hoá toàn bộ trạng thái hệ thống

### 🤝 Soft Skills

* Phân tích biểu đồ để tìm bottleneck
* Viết mô tả sự kiện rõ ràng khi nhận cảnh báo
* Tư duy quan sát hệ thống một cách chủ động thay vì phản ứng

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Không nhận được cảnh báo email

* **Mô tả**: Đã tạo Alarm và SNS nhưng không có email
* **Impact**: Không kịp phản ứng khi hệ thống vượt ngưỡng
* **Root Cause**: Email chưa xác nhận subscription từ SNS
* **Solution**: Vào email xác nhận liên kết trong email SNS
* **Result**: Email cảnh báo hoạt động đúng sau khi xác nhận
* **Lesson**: SNS cần xác thực trước khi gửi email

---

### Vấn đề 2: Không thấy log đẩy từ EC2 lên CloudWatch

* **Mô tả**: Log `/var/log/syslog` không xuất hiện
* **Impact**: Không giám sát được hành vi hệ điều hành
* **Root Cause**: Agent thiếu quyền IAM để push log
* **Solution**: Gán IAM Role với policy `CloudWatchAgentServerPolicy`
* **Result**: Log gửi được lên CloudWatch đầy đủ
* **Lesson**: Log Agent cần đủ quyền và đúng cấu hình file json

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Nhận diện và phản ứng tốt với cảnh báo tự động
* Cấu hình thành công luồng log từ EC2 về CloudWatch
* Hiểu rõ các khái niệm giám sát hiện đại trên cloud

### 🔄 What could be improved?

* Cần cấu hình thêm dashboard để trực quan hoá dễ hơn
* Tìm hiểu thêm về **Composite Alarms** để kết hợp nhiều điều kiện
* Có thể tích hợp CloudWatch với Lambda hoặc EventBridge để tự động hóa phản ứng

### 💡 Key Insights

* CloudWatch là công cụ không thể thiếu để vận hành hạ tầng AWS hiệu quả
* Cảnh báo sớm giúp bảo vệ hệ thống khỏi downtime hoặc chi phí vượt mức
* Logs không chỉ để debug mà còn là nguồn dữ liệu phân tích hành vi ứng dụng

