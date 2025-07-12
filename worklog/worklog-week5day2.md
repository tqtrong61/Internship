# 📘 Worklog - Ngày 10/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 10/06/2025
* **Thứ**: Thứ Ba
* **Tuần thực tập**: Tuần thứ 5/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 🛡️ + Cảm giác đang "bảo vệ cả hệ thống" khi tìm hiểu về DR

---

## 🎯 Mục tiêu ngày hôm nay

* Nắm vững khái niệm **Disaster Recovery** và các chiến lược DR trên AWS
* Làm quen với dịch vụ **AWS Elastic Disaster Recovery (DRS)**
* Cấu hình thử nghiệm DRS để sao chép và khôi phục hệ thống từ on-premises hoặc EC2
* Kiểm tra quá trình launch test và thực hiện failover

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu chiến lược DR và vai trò của AWS DRS ⏱️ 1.5 giờ

* **Mô tả**: Đọc các mô hình DR (Pilot Light, Warm Standby, Backup & Restore, Multi-site)
* **Kết quả**: Biết AWS DRS cung cấp khả năng sao chép liên tục, giảm RTO & RPO
* **Nguồn học**: [AWS Study Group - DRS](https://000100.awsstudygroup.com/vi/)

---

### 2. Cài đặt Agent và thiết lập Replication ⏱️ 2 giờ

* **Mô tả**: Cài AWS Replication Agent lên máy EC2 nguồn để chuẩn bị sao chép liên tục
* **Kết quả**: Hệ thống bắt đầu nhân bản block-level lên AWS
* **Tools/Tech**: AWS DRS, EC2, IAM, Replication Server, CloudWatch Logs
* **Lưu ý**: Cần mở port outbound và gán IAM Role đủ quyền để cài agent

---

### 3. Launch Test Instance từ hệ thống đã được replicate ⏱️ 1.5 giờ

* **Mô tả**: Dùng tính năng "Launch Test Instance" để kiểm tra tính khả dụng
* **Kết quả**: Test instance được tạo đúng theo bản sao lưu và hoạt động bình thường
* **Checklist**:

  * ✅ Network đúng subnet
  * ✅ Không ảnh hưởng production
  * ✅ SSH/RDP hoạt động ổn

---

### 4. Mô phỏng failover & phục hồi ⏱️ 2 giờ

* **Mô tả**: Tắt máy nguồn, thực hiện failover để chuyển traffic và khởi động bản sao
* **Kết quả**: DRS tạo máy EC2 mới để thay thế, RTO \~ vài phút
* **Sau đó**: Thực hiện "Recovery Cleanup" và re-protect lại nguồn

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Cài đặt và cấu hình AWS Elastic Disaster Recovery cho EC2
* Thao tác với replication agent và kiểm tra trạng thái sync
* Tạo test/failover instance và hiểu quy trình khôi phục
* Sử dụng tag, IAM và CloudWatch để giám sát DR

### 💡 Concepts & Theory

* **RTO (Recovery Time Objective)**: thời gian cần thiết để khôi phục dịch vụ
* **RPO (Recovery Point Objective)**: dữ liệu có thể mất trong quá trình phục hồi
* AWS DRS giúp tối ưu cả RTO và RPO với giá hợp lý
* DR Plan cần kết hợp kiểm thử định kỳ và chính sách bảo mật đi kèm

### 🤝 Soft Skills

* Lập kế hoạch rõ ràng khi tiến hành thử nghiệm DR
* Ghi chép từng bước khi launch test để đảm bảo tính tái lập
* Tư duy phản ứng nhanh trong tình huống hệ thống có sự cố

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Agent không thể kết nối AWS DRS

* **Mô tả**: Dù đã cài agent nhưng trạng thái luôn là “Not replicating”
* **Impact**: Không sao chép dữ liệu → DR thất bại
* **Root Cause**: Security Group chặn outbound traffic đến DRS endpoint
* **Solution**: Mở outbound port 443 và chỉ định đúng IAM Instance Profile
* **Result**: Agent hoạt động ổn định
* **Lesson**: Các dịch vụ DR cần **network thông thoáng & IAM chính xác**

---

### Vấn đề 2: Test Instance không khởi động đúng cấu hình

* **Mô tả**: Sau khi test launch, instance bị lỗi không gắn volume
* **Impact**: Không thể verify hệ thống đã sẵn sàng
* **Root Cause**: Thiếu snapshot hoặc bản sao chưa hoàn tất sync
* **Solution**: Kiểm tra trạng thái replication trước khi launch, delay vài phút
* **Result**: Khởi động thành công
* **Lesson**: Kiểm thử DR phải luôn có **buffer thời gian** để sync ổn định

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Làm chủ quy trình tạo DR system với AWS Elastic Disaster Recovery
* Hiểu cách test hệ thống mà không ảnh hưởng production
* Nắm rõ các chỉ số đánh giá hệ thống DR: RTO, RPO

### 🔄 What could be improved?

* Nên thử nghiệm với on-premises giả lập bằng VirtualBox + agent
* Tạo DR Plan có automation bằng CloudFormation hoặc AWS CLI
* Cấu hình SNS để thông báo trạng thái DR đến email nhóm IT

### 💡 Key Insights

* Disaster Recovery là **bắt buộc** cho bất kỳ hệ thống nào có dữ liệu quan trọng
* AWS DRS giúp xây dựng DR plan chuyên nghiệp mà không cần phần cứng riêng
* Việc kiểm thử định kỳ DR là cách duy nhất để đảm bảo hệ thống **thực sự sẵn sàng khi cần**

Worklog created by: Tran Quang Trong
