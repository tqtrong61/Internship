# 📘 Worklog - Ngày 22/05/2025

## 📅 Thông tin cơ bản

* **Ngày**: 22/05/2025
* **Thứ**: Thứ Năm
* **Tuần thực tập**: Tuần thứ 2/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 🌐 + Hào hứng với khái niệm quản lý tên miền và điều hướng toàn cầu

---

## 🎯 Mục tiêu ngày hôm nay

* Hiểu cơ bản về Domain Name System (DNS) và vai trò của Amazon Route 53
* Đăng ký tên miền và cấu hình các bản ghi (record sets)
* Tích hợp Route 53 với website tĩnh trên S3 và CloudFront
* Tìm hiểu mô hình **hybrid DNS** và forward DNS queries về local/on-premises

---

## 💼 Công việc đã thực hiện

### 1. Hiểu về DNS và kiến trúc Route 53 ⏱️ 1.5 giờ

* **Mô tả**: Nắm được khái niệm DNS Resolver, Authoritative DNS, TTL, các loại bản ghi (A, AAAA, CNAME…)
* **Kết quả**: Có nền tảng về cách Route 53 hoạt động như DNS quản lý toàn cầu
* **Tools/Tech**: Amazon Route 53, DNS Lookup Tool
* **Links**: [Hướng dẫn AWS Study Group](https://000010.awsstudygroup.com/vi/)

---

### 2. Đăng ký domain và trỏ về S3/CloudFront ⏱️ 2 giờ

* **Mô tả**: Đăng ký domain `fcjdemo.cloud`, trỏ bản ghi A → S3 static website hoặc CloudFront distribution
* **Kết quả**: Website truy cập được qua tên miền riêng
* **Tools/Tech**: Route 53 Hosted Zones, CloudFront, S3
* **Lệnh tiêu biểu** (CLI):

  ```bash
  aws route53 change-resource-record-sets \
    --hosted-zone-id ZXXXXXX \
    --change-batch file://recordset.json
  ```

---

### 3. Tìm hiểu DNS lai (hybrid DNS) ⏱️ 2 giờ

* **Mô tả**: Nghiên cứu cách Route 53 Resolver có thể forward DNS query về on-premises thông qua outbound endpoint
* **Kết quả**: Hiểu vai trò trong các hệ thống lai (hybrid network) như VPC ↔ On-prem
* **Tools/Tech**: Route 53 Resolver, VPC, Outbound/Inbound Endpoints

---

### 4. Cấu hình cảnh báo nếu DNS fail (health check) ⏱️ 1 giờ

* **Mô tả**: Tạo health check để kiểm tra endpoint, tự động điều hướng nếu server chính mất kết nối
* **Kết quả**: Tăng tính sẵn sàng cho hệ thống DNS
* **Tools/Tech**: Route 53 Health Checks, Failover Routing

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Quản lý domain với Route 53 và cấu hình bản ghi DNS
* Tích hợp Route 53 với CloudFront để truy cập web qua tên miền tuỳ chỉnh
* Sử dụng health check và failover routing để tăng tính sẵn sàng
* Khám phá mô hình DNS Resolver và forwarding

### 💡 Concepts & Theory

* Route 53 = DNS + Domain Registrar + Health Check + Traffic Policy
* Record Set = tập hợp các bản ghi giúp định danh tài nguyên
* TTL (Time to Live) ảnh hưởng đến tốc độ cập nhật DNS
* Hybrid DNS dùng trong doanh nghiệp có hệ thống kết hợp giữa cloud và on-premises

### 🤝 Soft Skills

* Cẩn thận khi thao tác với domain vì ảnh hưởng truy cập người dùng
* Kỹ năng đọc lỗi từ công cụ DNS Lookup và Debug
* Tư duy kiến trúc mạng khi cấu hình failover hoặc hybrid

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Trỏ domain xong nhưng không truy cập được

* **Mô tả**: Domain đăng ký thành công nhưng trình duyệt báo lỗi
* **Impact**: Người dùng không thể truy cập website
* **Root Cause**: Bản ghi chưa cập nhật đúng endpoint CloudFront hoặc thiếu bản ghi A/AAAA
* **Solution**: Kiểm tra Hosted Zone, chỉnh lại bản ghi A và thêm `alias` nếu dùng CloudFront
* **Result**: Domain hoạt động đúng sau 5-10 phút propagate DNS
* **Lesson**: Cần kiên nhẫn chờ thời gian cập nhật DNS và xác minh lại cấu hình

---

### Vấn đề 2: Không hiểu rõ DNS forward trong hybrid model

* **Mô tả**: Khó hình dung route DNS quay về hệ thống on-prem
* **Impact**: Chưa rõ use-case thật sự và cấu hình
* **Root Cause**: Chưa có môi trường hybrid thật để thực hành
* **Solution**: Đọc thêm tài liệu chính thức AWS + sơ đồ mô hình hybrid DNS
* **Result**: Hiểu cơ bản cách dùng resolver endpoints để forward DNS
* **Lesson**: DNS Resolver là phần nâng cao, cần vẽ sơ đồ để dễ hiểu

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Tự tay cấu hình domain → CloudFront thành công
* Biết dùng record sets, TTL và health checks
* Có cái nhìn tổng thể về kiến trúc DNS của AWS

### 🔄 What could be improved?

* Nên tạo thêm domain thứ hai để test failover scenario
* Cần áp dụng Route 53 traffic policies (latency, geolocation) để hiểu sâu hơn
* Thực hành nhiều hơn với resolver endpoints nếu có lab hybrid

### 💡 Key Insights

* Route 53 không chỉ là DNS, mà còn là công cụ định tuyến thông minh và giám sát truy cập
* Việc điều hướng qua domain tùy chỉnh giúp chuyên nghiệp hóa sản phẩm
* Hybrid DNS phù hợp cho doanh nghiệp vừa chuyển dịch lên cloud từng phần

Worklog created by: Tran Quang Trong