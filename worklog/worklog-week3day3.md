# 📘 Worklog - Ngày 28/05/2025

## 📅 Thông tin cơ bản

* **Ngày**: 28/05/2025
* **Thứ**: Thứ Tư
* **Tuần thực tập**: Tuần thứ 3/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 🧱 + Hào hứng khi có thể “code hoá” toàn bộ hạ tầng

---

## 🎯 Mục tiêu ngày hôm nay

* Tìm hiểu về Infrastructure as Code và công cụ AWS CloudFormation
* Viết template đầu tiên để tạo S3 Bucket và EC2 Instance
* Tìm hiểu cách sử dụng Parameters, Mappings, Outputs trong YAML
* Thực hành cập nhật Stack và rollback khi gặp lỗi

---

## 💼 Công việc đã thực hiện

### 1. Làm quen với CloudFormation và khái niệm Stack ⏱️ 1.5 giờ

* **Mô tả**: Học cách CloudFormation quản lý hạ tầng thông qua Stack và Template
* **Kết quả**: Hiểu cách hoạt động của declarative IaC trên AWS
* **Tools/Tech**: AWS Console, CloudFormation Dashboard
* **Links**: [CloudFormation Study Guide](https://000037.awsstudygroup.com/vi/)

---

### 2. Viết YAML template tạo S3 Bucket và EC2 Instance ⏱️ 2.5 giờ

* **Mô tả**: Tạo file `template.yml` định nghĩa tài nguyên, test deploy stack
* **Kết quả**: Stack chạy thành công, tạo đúng tên bucket và EC2 t2.micro
* **Tools/Tech**: YAML, CloudFormation Template, EC2, S3
* **Links**: [Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)

---

### 3. Bổ sung Parameters và Outputs cho Template ⏱️ 1 giờ

* **Mô tả**: Cho phép người dùng truyền biến như InstanceType, BucketName và hiển thị output PublicIP
* **Kết quả**: Template linh hoạt hơn, dễ tái sử dụng
* **Tools/Tech**: Parameters, Outputs, Ref, Fn::Sub

---

### 4. Thử cập nhật Stack và xử lý lỗi rollback ⏱️ 1 giờ

* **Mô tả**: Cố tình cấu hình sai để test rollback và sửa template cho đúng
* **Kết quả**: Nắm được cơ chế rollback tự động khi deploy thất bại
* **Tools/Tech**: CloudFormation Stack Events, Template Validator

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Viết CloudFormation template YAML định nghĩa hạ tầng
* Sử dụng Parameters và Outputs để template có thể tái sử dụng
* Deploy, update và delete Stack từ Console

### 💡 Concepts & Theory

* **Infrastructure as Code** (IaC) giúp mô hình hoá hạ tầng bằng tệp tin mã nguồn
* CloudFormation là declarative tool: mô tả trạng thái mong muốn, AWS thực thi phần còn lại
* Stack = 1 nhóm tài nguyên được tạo/cập nhật/xoá cùng nhau

### 🤝 Soft Skills

* Làm việc cẩn thận với YAML vì dễ mắc lỗi indentation
* Quản lý version của template để rollback hiệu quả
* Tư duy kiểm soát thay đổi hạ tầng như code source

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Lỗi khi deploy Stack do sai cú pháp YAML

* **Mô tả**: Stack fail vì indentation không đúng
* **Impact**: Không khởi tạo được tài nguyên
* **Root Cause**: YAML sensitive với khoảng trắng và cấp bậc
* **Solution**: Dùng VSCode có plugin YAML + validate template online
* **Result**: Template chạy mượt sau khi fix cú pháp
* **Lesson**: Luôn validate YAML trước khi deploy

---

### Vấn đề 2: Stack rollback mà không rõ nguyên nhân

* **Mô tả**: Stack bị xoá tự động do lỗi nhưng không biết tại sao
* **Impact**: Mất thời gian debug
* **Root Cause**: EC2 Instance thiếu IAM Role cần thiết
* **Solution**: Đọc kỹ phần Events, sửa lại template bổ sung Role
* **Result**: Stack tạo thành công sau khi fix
* **Lesson**: Log trong tab "Events" rất quan trọng để tìm nguyên nhân

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Viết được template CloudFormation đầu tiên từ đầu đến cuối
* Biết cách tạo tham số, truyền biến và xuất thông tin output
* Hiểu rõ cách rollback hoạt động giúp tăng tính an toàn

### 🔄 What could be improved?

* Nên chia template thành Nested Stack để quản lý tốt hơn
* Thử triển khai Stack từ AWS CLI thay vì Console
* Kết hợp với Git để versioning template dễ theo dõi

### 💡 Key Insights

* IaC là bước đầu tiên để tự động hóa toàn bộ hạ tầng AWS
* CloudFormation giúp triển khai, nhân bản, rollback môi trường dễ dàng và an toàn
* Template tốt = hạ tầng có thể chạy đi chạy lại nhiều lần mà vẫn đúng như thiết kế

Worklog created by: Tran Quang Trong
Next review: 29/05/2025
