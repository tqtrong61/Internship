# 📘 Worklog - Ngày 25/06/2025

## 📅 Thông tin cơ bản

* **Ngày**: 25/06/2025
* **Thứ**: Thứ Tư
* **Tuần thực tập**: Tuần thứ 7/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 📉 + Quan tâm về rủi ro mất dữ liệu và giải pháp phát hiện sớm bất thường

---

## 🎯 Mục tiêu ngày hôm nay

* Tìm hiểu cơ chế sao lưu ổ đĩa EBS và vai trò của snapshot
* Thiết lập theo dõi **bất thường số lượng snapshot** với Amazon CloudWatch + EventBridge
* Tạo cảnh báo khi số lượng snapshot tăng/giảm bất thường (dấu hiệu nguy cơ hoặc lỗi automation)
* Tích hợp cảnh báo vào SNS để gửi email

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu cơ chế sao lưu EBS và snapshot ⏱️ 1 giờ

* **Mô tả**: Hiểu snapshot là bản sao cấp block của EBS volume, lưu vào S3 nội bộ của AWS
* **Kết quả**: Biết snapshot rất quan trọng với kế hoạch backup & DR của hệ thống EC2

---

### 2. Viết script dùng AWS CLI để thống kê số lượng snapshot theo ngày ⏱️ 1.5 giờ

* **Mô tả**: Tạo script Python + AWS CLI liệt kê snapshot theo tag, theo thời gian tạo
* **Kết quả**: In được số lượng snapshot hiện có theo từng volume
* **Code mẫu**:

  ```bash
  aws ec2 describe-snapshots --owner-ids self \
    --query 'Snapshots[?StartTime>=`2025-05-30T00:00:00Z`]' \
    --output table
  ```

---

### 3. Thiết lập CloudWatch Metric Custom ⏱️ 2 giờ

* **Mô tả**: Tạo metric tùy chỉnh để đưa số lượng snapshot mỗi ngày vào CloudWatch
* **Kết quả**: Có thể vẽ biểu đồ biến động snapshot mỗi ngày
* **Cách làm**:

  * Dùng AWS CLI push custom metric:

    ```bash
    aws cloudwatch put-metric-data \
      --namespace "EBSBackupMonitoring" \
      --metric-name "SnapshotCount" \
      --value 5 \
      --dimensions VolumeID=vol-01234
    ```

---

### 4. Cấu hình cảnh báo khi có biến động bất thường ⏱️ 1.5 giờ

* **Mô tả**: Tạo alarm nếu snapshot Count > 10 hoặc < 1
* **Kết quả**: Khi hệ thống tạo quá nhiều snapshot hoặc không tạo snapshot nào → cảnh báo gửi qua email
* **Tools**: CloudWatch Alarm + SNS

---

### 5. Viết EventBridge Rule giám sát snapshot được tạo tự động ⏱️ 1 giờ

* **Mô tả**: Tạo Rule bắt sự kiện `CreateSnapshot` từ AWS API
* **Kết quả**: Theo dõi được mọi hành động tạo snapshot, ai tạo, khi nào
* **Log**: Đẩy log vào CloudWatch để truy vết về sau

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Truy vấn và phân tích snapshot bằng AWS CLI
* Gửi custom metric vào CloudWatch và hiển thị bằng biểu đồ
* Tạo CloudWatch Alarm với điều kiện số lượng vượt ngưỡng
* Bắt sự kiện với EventBridge và tích hợp SNS cảnh báo

### 💡 Concepts & Theory

* Snapshot tăng bất thường có thể là:

  * 🔺 Dấu hiệu automation bị lỗi → tạo snapshot liên tục
  * 🔻 Không có snapshot → mất an toàn dữ liệu
* Việc giám sát số lượng snapshot giúp phát hiện vấn đề backup sớm
* CloudWatch không theo dõi snapshot theo mặc định → cần custom metric

### 🤝 Soft Skills

* Khả năng tự tìm giải pháp khi AWS không cung cấp sẵn metric
* Quản lý thời gian làm việc hợp lý giữa viết script – test – gắn alarm
* Tư duy cảnh báo từ nhiều hướng: kỹ thuật + vận hành + bảo mật

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Không thấy metric SnapshotCount trên CloudWatch

* **Mô tả**: Dù đã push metric nhưng dashboard không hiển thị
* **Impact**: Không thể tạo cảnh báo
* **Root Cause**: Quên đặt namespace hoặc đặt sai dimension
* **Solution**: Kiểm tra lại namespace + push lại metric
* **Result**: Metric xuất hiện sau vài phút
* **Lesson**: Namespace và dimension rất quan trọng trong custom metrics

---

### Vấn đề 2: Không nhận được email cảnh báo

* **Mô tả**: CloudWatch Alarm chuyển trạng thái nhưng không có email
* **Impact**: Không có người nào nhận được cảnh báo khi có sự cố
* **Root Cause**: SNS subscription chưa được confirm
* **Solution**: Check email inbox → Confirm link SNS
* **Result**: Cảnh báo đến đầy đủ
* **Lesson**: Đừng quên bước xác nhận email SNS khi test cảnh báo

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Đã tự động hóa giám sát được một phần quan trọng nhưng thường bị bỏ quên: snapshot
* Có hệ thống cảnh báo nếu số lượng snapshot có dấu hiệu bất thường
* Làm quen với custom metric và EventBridge rule

### 🔄 What could be improved?

* Nên kết hợp thêm Lambda function để tự động xóa snapshot cũ
* Tích hợp gửi cảnh báo qua Slack hoặc Telegram cho team DevOps
* Viết Terraform để tái sử dụng toàn bộ hệ thống giám sát snapshot

### 💡 Key Insights

* AWS không theo dõi mọi thứ sẵn có → bạn cần tự tạo các chỉ số cần thiết
* EBS snapshot tuy nhỏ, nhưng giám sát kỹ sẽ giúp tránh thảm họa mất dữ liệu
* Việc cảnh báo phải đi đôi với kế hoạch phản ứng nhanh → cần quy trình rõ ràng

Worklog created by: Tran Quang Trong