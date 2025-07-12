# 📘 Worklog - Ngày 18/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 18/06/2025
* **Thứ**: Thứ Tư
* **Tuần thực tập**: Tuần thứ 6/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 📊 + Hứng thú khi lần đầu trực quan hóa hệ thống AWS bằng Grafana

---

## 🎯 Mục tiêu ngày hôm nay

* Hiểu tổng quan kiến trúc giám sát hệ thống với **Amazon CloudWatch**
* Kết nối **Amazon Managed Grafana** với dữ liệu CloudWatch để trực quan hóa
* Thiết lập dashboard theo dõi các chỉ số EC2, RDS và Lambda
* Cấu hình cảnh báo (alarm) và nhận thông báo qua SNS

---

## 💼 Công việc đã thực hiện

### 1. Ôn lại kiến thức về Amazon CloudWatch ⏱️ 1 giờ

* **Mô tả**: Tìm hiểu lại các thành phần chính: Logs, Metrics, Alarms, Dashboards
* **Kết quả**: Nắm được cách CloudWatch thu thập dữ liệu từ các dịch vụ AWS

---

### 2. Khởi tạo Amazon Managed Grafana workspace ⏱️ 1.5 giờ

* **Mô tả**: Tạo workspace Grafana được quản lý bởi AWS
* **Kết quả**: Có UI quản trị trực quan truy cập qua link riêng
* **Tools/Tech**: Amazon Managed Grafana, IAM Identity Center, CloudWatch

---

### 3. Kết nối dữ liệu từ CloudWatch vào Grafana ⏱️ 1.5 giờ

* **Mô tả**: Gán permission cho Grafana workspace để đọc dữ liệu CloudWatch
* **Kết quả**: Grafana truy xuất được metric từ EC2, Lambda, RDS
* **Cấu hình chính**:

  * Tạo service role với `AmazonGrafanaCloudWatchAccess`
  * Add data source: CloudWatch → Region + Namespace

---

### 4. Tạo Dashboard tùy chỉnh giám sát EC2 & Lambda ⏱️ 2 giờ

* **Mô tả**: Thiết kế dashboard theo dõi CPU, Memory EC2 và request count Lambda
* **Kết quả**: Dashboard trực quan, dễ theo dõi, có nhiều biểu đồ dạng dòng, cột
* **Widgets**:

  * 📈 EC2: `CPUUtilization`, `NetworkIn/Out`
  * ⚙️ Lambda: `Invocations`, `Errors`, `Duration`

---

### 5. Thiết lập CloudWatch Alarms và gửi cảnh báo qua SNS ⏱️ 1 giờ

* **Mô tả**: Đặt cảnh báo khi CPU EC2 > 80% trong 5 phút
* **Kết quả**: Khi vượt ngưỡng sẽ gửi email đến nhóm DevOps
* **Tools/Tech**: CloudWatch Alarm, SNS Topic, Subscription Email

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Tạo và quản lý workspace Amazon Managed Grafana
* Kết nối Grafana với CloudWatch để hiển thị real-time metrics
* Thiết lập dashboard theo dõi hệ thống trực quan, thân thiện
* Tạo cảnh báo sử dụng CloudWatch Alarms + SNS Notification

### 💡 Concepts & Theory

* **Observability** = Logs + Metrics + Traces → CloudWatch cung cấp đủ cả 3
* **Grafana** là công cụ mã nguồn mở để visualize dữ liệu giám sát
* So sánh: CloudWatch dashboard = cơ bản; Grafana = nâng cao, đẹp, tùy chỉnh tốt
* Cảnh báo cần dựa vào business logic chứ không chỉ technical threshold

### 🤝 Soft Skills

* Thiết kế dashboard dễ hiểu cho cả technical và non-technical users
* Diễn giải ý nghĩa các chỉ số vận hành như `CPUUtilization`, `Error Rate`
* Chủ động kiểm thử bằng cách “giả lập lỗi” để kiểm tra cảnh báo

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Grafana không truy cập được CloudWatch

* **Mô tả**: Không hiển thị dữ liệu dù đã thêm data source
* **Impact**: Dashboard trống → không thể giám sát
* **Root Cause**: Service Role gán sai hoặc thiếu quyền
* **Solution**: Tạo lại IAM Role với policy `AmazonGrafanaCloudWatchAccess`, gán đúng workspace
* **Result**: Truy cập metric thành công
* **Lesson**: IAM chính xác là điều kiện bắt buộc khi tích hợp dịch vụ

---

### Vấn đề 2: Không nhận được cảnh báo từ CloudWatch Alarm

* **Mô tả**: Dù CPU vượt ngưỡng nhưng không có email
* **Impact**: Không thể kiểm soát kịp thời khi hệ thống có vấn đề
* **Root Cause**: SNS topic chưa có subscription xác nhận email
* **Solution**: Kiểm tra lại email, click xác nhận trong inbox
* **Result**: Nhận email cảnh báo thành công
* **Lesson**: Luôn kiểm tra trạng thái “confirmed” của SNS subscription

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Đã triển khai thành công hệ thống giám sát real-time bằng CloudWatch + Grafana
* Tạo dashboard dễ nhìn, hữu ích cho vận hành hệ thống
* Hiểu rõ quy trình cảnh báo từ metrics → alarm → SNS

### 🔄 What could be improved?

* Nên tích hợp thêm các dịch vụ khác: RDS, ALB, S3 Access logs
* Học thêm cách viết query trong Grafana (Metric Math, PromQL)
* Tìm hiểu tích hợp với PagerDuty, Slack hoặc Opsgenie để cảnh báo nhanh hơn

### 💡 Key Insights

* Kết hợp CloudWatch + Grafana tạo ra công cụ giám sát mạnh mẽ và linh hoạt
* Việc giám sát không chỉ là theo dõi — mà còn là khả năng phản ứng sớm
* Cảnh báo chỉ hiệu quả khi có kế hoạch xử lý và người nhận rõ ràng

Worklog created by: Tran Quang Trong