# 📊 **TỔNG KẾT TUẦN 6 THỰC TẬP (17/06 - 22/06/2025)**

**Thực tập sinh**: Trần Quang Trọng
## 🎯 **Những kiến thức đã học được**

### 1. **Quan sát hệ thống (Observability) với Amazon CloudWatch và Grafana**

* Nắm rõ các khái niệm cốt lõi trong giám sát hệ thống: `Logs`, `Metrics`, `Traces`, `Alarms`, `Dashboards`
* Thiết lập **Amazon Managed Grafana** và kết nối thành công với dữ liệu từ CloudWatch
* Hiểu cách tạo **Dashboard trực quan hóa** dữ liệu từ EC2, Lambda, RDS
* Thiết lập **CloudWatch Alarms** để phát hiện bất thường và gửi cảnh báo qua SNS
* So sánh giữa Dashboard mặc định của CloudWatch và sự linh hoạt của Grafana

### 2. **Tự động hóa sao lưu với AWS Backup**

* Hiểu được quy trình và thành phần chính của AWS Backup: Vault, Plan, Recovery Point
* Tạo kế hoạch sao lưu định kỳ cho EC2 và RDS, có **retention policy** và mã hóa bằng KMS
* Thực hành **khôi phục EC2** từ Recovery Point, nắm được các rủi ro và thao tác cần thiết
* Hiểu vai trò của **tag-based resource selection** khi gán tài nguyên vào backup plan

---

## 💡 **Kỹ năng và kinh nghiệm rút ra**

### 🔧 **Technical Skills**

* Tích hợp các dịch vụ AWS với nhau: CloudWatch ↔ Grafana, Backup ↔ EC2/RDS
* Thiết lập hệ thống giám sát **real-time** hỗ trợ vận hành
* Tự động hóa bảo vệ dữ liệu và phục hồi trong tình huống khẩn cấp
* Biết kiểm tra log, trạng thái job và quyền IAM khi gặp lỗi

### 🤝 **Soft Skills**

* Tư duy hệ thống: hiểu quy trình từ giám sát → cảnh báo → phản ứng
* Rèn luyện tính **cẩn thận** khi thao tác với dữ liệu và tài nguyên thật
* Biết cách **giao tiếp kỹ thuật** thông qua dashboard dễ hiểu cho nhiều đối tượng

---

## 🚧 **Khó khăn gặp phải và cách giải quyết**

| **Vấn đề**                                | **Nguyên nhân gốc**                             | **Bài học rút ra**                                                             |
| ----------------------------------------- | ----------------------------------------------- | ------------------------------------------------------------------------------ |
| Grafana không lấy được dữ liệu CloudWatch | IAM Role chưa có đủ quyền                       | Phải nắm rõ quyền truy cập khi tích hợp giữa các dịch vụ AWS                   |
| Không nhận được cảnh báo từ Alarm         | SNS topic chưa được xác nhận email subscription | Luôn kiểm tra trạng thái của SNS Subscription                                  |
| EC2 không hiện trong AWS Backup Plan      | Thiếu tag `Backup=true`                         | Gán tag đúng giúp hệ thống tự động nhận diện tài nguyên                        |
| Restore EC2 bị lỗi thiếu quyền            | Role IAM chưa có `ec2:RunInstances`             | Backup không chỉ là lưu trữ, mà còn phải đảm bảo khả năng khôi phục đúng quyền |

---

## 🔄 **Gợi ý cải thiện cho tuần tới**

* Bổ sung cảnh báo nếu **Backup job thất bại** thông qua CloudWatch Alarm
* Tích hợp thêm các hệ thống cảnh báo như Slack, PagerDuty để phản ứng kịp thời
* Học thêm về **Infrastructure as Code (IaC)** để viết script backup với AWS CLI hoặc Terraform
* Tìm hiểu backup **cross-region** cho nhu cầu DR (Disaster Recovery)

---

## 🧠 **Key Takeaways**

* **Giám sát** và **backup** là hai trụ cột không thể thiếu trong vận hành hệ thống AWS ổn định
* Chỉ khi có khả năng **khôi phục**, thì backup mới thật sự có giá trị
* Giám sát hiệu quả không chỉ là nhìn số liệu – mà là biết khi nào cần hành động

Worklog created by: Tran Quang Trong
