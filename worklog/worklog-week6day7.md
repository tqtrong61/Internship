# 📘 Worklog - Ngày 22/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 22/06/2025
* **Thứ**: Thứ Tư
* **Tuần thực tập**: Tuần thứ 6/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 💾 + Yên tâm vì biết dữ liệu đã được tự động sao lưu

---

## 🎯 Mục tiêu ngày hôm nay

* Hiểu được tầm quan trọng của backup trong bảo vệ dữ liệu
* Làm quen với dịch vụ **AWS Backup** và các loại tài nguyên được hỗ trợ
* Thiết lập kế hoạch sao lưu tự động cho EC2 và RDS
* Khôi phục dữ liệu từ backup point và kiểm tra tính sẵn sàng

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu tổng quan về AWS Backup và các thành phần chính ⏱️ 1 giờ

* **Mô tả**: Đọc tài liệu để nắm khái niệm Vault, Backup Plan, Recovery Point
* **Kết quả**: Biết cách hoạt động của dịch vụ AWS Backup và ứng dụng thực tế

---

### 2. Tạo Vault lưu trữ bản backup ⏱️ 0.5 giờ

* **Mô tả**: Tạo AWS Backup Vault mới có bật mã hóa bằng AWS KMS
* **Kết quả**: Có không gian lưu trữ riêng cho backup, phân quyền rõ ràng
* **Ghi chú**: Vault có thể kích hoạt **access control** bằng IAM hoặc KMS Key policy

---

### 3. Thiết lập Backup Plan cho EC2 ⏱️ 2 giờ

* **Mô tả**: Định nghĩa lịch backup mỗi ngày 1 lần, giữ lại 7 bản gần nhất
* **Kết quả**: EC2 tự động được sao lưu hàng ngày vào 2h sáng
* **Thông số**:

  * Frequency: Daily
  * Retention: 7 days
  * Start time: 2:00 AM
  * Resource Type: EC2
  * Vault: `intern-backup-vault`

---

### 4. Thêm RDS vào kế hoạch backup ⏱️ 1.5 giờ

* **Mô tả**: Attach RDS vào backup plan đã có sẵn
* **Kết quả**: Cơ sở dữ liệu được sao lưu định kỳ giống như EC2
* **Tips**: RDS có cơ chế snapshot riêng, nhưng AWS Backup quản lý tập trung hơn

---

### 5. Thử nghiệm khôi phục dữ liệu từ Recovery Point ⏱️ 2 giờ

* **Mô tả**: Xoá EC2 test instance rồi khôi phục lại từ bản backup gần nhất
* **Kết quả**: Instance được restore đầy đủ với AMI mới
* **Lưu ý**: EC2 khôi phục sẽ có Instance ID mới và cần cấu hình lại security group nếu cần

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Tạo Vault, Plan và gán tài nguyên EC2/RDS vào backup policy
* Cấu hình backup retention, schedule và encryption
* Sử dụng giao diện AWS Backup để quản lý recovery points
* Khôi phục dữ liệu một cách thủ công hoặc theo kế hoạch định kỳ

### 💡 Concepts & Theory

* **Recovery Point**: snapshot/tệp backup được tạo từ tài nguyên
* **Retention Policy**: xác định thời gian giữ bản backup để tối ưu chi phí
* **Vault**: nơi lưu trữ và kiểm soát truy cập backup
* **Cross-Region Backup**: hỗ trợ sao lưu sang vùng khác để dự phòng khu vực

### 🤝 Soft Skills

* Thực hành cẩn trọng: đảm bảo không xóa nhầm tài nguyên thật
* Ghi chú rõ ràng các bước backup – restore để tái sử dụng cho hệ thống khác
* Thói quen kiểm tra logs, trạng thái backup job mỗi ngày

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: EC2 không hiển thị trong backup plan

* **Mô tả**: Không tìm thấy EC2 khi chọn resource
* **Impact**: Không gán được vào plan → không backup được
* **Root Cause**: EC2 chưa được gắn tag phù hợp
* **Solution**: Gắn tag `Backup=true` để AWS Backup nhận diện
* **Result**: EC2 hiển thị và thêm được vào plan
* **Lesson**: AWS Backup hoạt động dựa trên **tag-based resource mapping**

---

### Vấn đề 2: Khôi phục EC2 bị lỗi do thiếu quyền

* **Mô tả**: Restore EC2 báo lỗi permission khi tạo instance
* **Impact**: Không thể test quá trình recover
* **Root Cause**: Role sử dụng không có quyền `ec2:RunInstances`
* **Solution**: Cập nhật IAM Role theo policy chuẩn
* **Result**: Khôi phục thành công
* **Lesson**: Phân quyền chính xác là yêu cầu quan trọng khi thực hiện backup/restore

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Triển khai thành công backup tự động cho cả EC2 và RDS
* Biết cách khôi phục tài nguyên từ bản backup thực tế
* Hiểu được cách hoạt động của Vault và kế hoạch giữ liệu lâu dài

### 🔄 What could be improved?

* Nên tạo thêm cảnh báo khi backup job thất bại (dùng CloudWatch Alarm)
* Thử dùng backup cross-region để dự phòng thảm họa vùng
* Viết automation backup bằng AWS CLI hoặc Terraform

### 💡 Key Insights

* AWS Backup giúp chuẩn hóa và tự động hóa quy trình bảo vệ dữ liệu
* Cần kiểm tra thường xuyên các bản sao lưu và lên kế hoạch kiểm thử phục hồi định kỳ
* Gắn tag đúng sẽ giúp quản lý backup tập trung và linh hoạt hơn

Worklog created by: Tran Quang Trong
Next review: 23/06/2025