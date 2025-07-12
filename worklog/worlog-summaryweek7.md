# 📊 **TỔNG KẾT TUẦN 7 THỰC TẬP (23/06 – 25/06/2025)**

**Thực tập sinh**: Trần Quang Trọng
## 🎯 **Những kiến thức đã học được**

### 1. **Truy cập bảo mật EC2 bằng Systems Manager Session Manager**

* Hiểu kiến trúc hoạt động của **Session Manager**: EC2 ↔ SSM Agent ↔ IAM ↔ Console/CLI
* Triển khai thành công truy cập EC2 mà **không cần SSH key và không mở cổng 22**
* Truy cập EC2 từ nhiều cách: AWS Console, CLI, CloudShell
* Ghi log toàn bộ phiên session vào CloudWatch và S3 để phục vụ kiểm tra, giám sát
* Nâng cao khả năng bảo mật và kiểm soát hoạt động truy cập hệ thống

### 2. **Giám sát bất thường trong sao lưu EBS snapshot**

* Hiểu snapshot là phương thức backup block-level của EBS volume
* Viết script truy vấn snapshot bằng AWS CLI để thống kê số lượng theo ngày, theo tag
* Tự tạo **custom CloudWatch metrics** để theo dõi biến động snapshot hàng ngày
* Thiết lập **CloudWatch Alarm** cảnh báo khi snapshot tăng bất thường (dấu hiệu lỗi) hoặc giảm về 0 (nguy cơ mất dữ liệu)
* Tạo **EventBridge Rule** bắt các sự kiện `CreateSnapshot`, lưu log để điều tra nếu cần

---

## 💡 **Kỹ năng và kinh nghiệm rút ra**

### 🔧 **Technical Skills**

* Sử dụng Systems Manager để truy cập và kiểm soát EC2 từ xa một cách bảo mật
* Tạo custom metric và automation cảnh báo bằng CloudWatch + SNS
* Sử dụng EventBridge để theo dõi hành vi tạo snapshot trong thời gian thực
* Cấu hình ghi log đầy đủ cho phiên truy cập và cảnh báo, đảm bảo **visibility**

### 🤝 **Soft Skills**

* Phân tích tình huống bảo mật và tìm ra giải pháp thay thế SSH truyền thống
* Có tư duy giám sát chủ động (proactive monitoring) với cảnh báo theo logic hoạt động
* Tư duy quản lý sự cố toàn diện: từ phát hiện → cảnh báo → phản ứng → kiểm tra log

---

## 🚧 **Khó khăn gặp phải và cách khắc phục**

| **Vấn đề**                               | **Nguyên nhân**                                       | **Giải pháp / Bài học rút ra**                                                 |
| ---------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------------------------ |
| EC2 không hiển thị trong Systems Manager | Thiếu IAM Role hoặc chưa cài SSM Agent                | Cấu hình lại Role `AmazonSSMManagedInstanceCore`, đảm bảo Agent đang hoạt động |
| Không ghi log session truy cập EC2       | Thiếu quyền ghi log hoặc chưa cấu hình đúng log group | Phải kiểm tra kỹ CloudWatch log group + IAM policy                             |
| Không thấy metric custom SnapshotCount   | Namespace hoặc dimension bị sai                       | Kiểm tra lại tên metric, namespace và định dạng push dữ liệu                   |
| Không nhận được cảnh báo từ SNS          | Chưa xác nhận subscription email                      | Luôn xác nhận email và kiểm tra trạng thái “Confirmed” của SNS subscription    |

---

## 🔄 **Gợi ý cải thiện cho tuần tới**

* Viết script tự động gán IAM Role, bật logging và thiết lập cấu hình chuẩn cho EC2
* Tích hợp thêm CloudTrail để theo dõi toàn bộ lịch sử truy cập và hành động người dùng
* Viết Lambda để dọn snapshot cũ, tránh chi phí lưu trữ tăng không kiểm soát
* Tìm hiểu và thực hành quản lý cấu hình bằng **Infrastructure as Code (Terraform/CloudFormation)**

---

## 🧠 **Key Takeaways**

* **Session Manager** giúp loại bỏ hoàn toàn sự phụ thuộc vào SSH key – một bước tiến lớn trong bảo mật
* Các chỉ số quan trọng như snapshot count không được AWS giám sát mặc định → **cần chủ động tạo và theo dõi**
* Hệ thống giám sát chỉ hiệu quả khi có **cảnh báo đúng**, **log đầy đủ** và **quy trình phản ứng rõ ràng**
* Bảo mật không đến từ sự phức tạp – mà đến từ **kiểm soát chặt chẽ và minh bạch hành vi hệ thống**

Worklog created by: Tran Quang Trong