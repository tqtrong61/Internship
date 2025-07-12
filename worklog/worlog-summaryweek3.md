# 📊 **Tổng kết Tuần 3 Thực Tập (Tuần từ 26/05/2025 - 30/05/2025)**

**Tên thực tập sinh**: Trần Quang Trọng
**Thời gian thực tập**: Tuần thứ 3/12

---

## 🎯 **Tổng quan nội dung học trong tuần**

### 1. **Infrastructure as Code (IaC) với AWS CloudFormation**

* Làm quen với khái niệm Stack và cách CloudFormation hoạt động theo hướng mô tả trạng thái mong muốn (declarative).
* Viết template YAML để tạo các tài nguyên như **S3 bucket**, **EC2 instance**, và sử dụng các thành phần như `Parameters`, `Outputs`, `Ref`, `Fn::Sub` để nâng cao tính linh hoạt và tái sử dụng.
* Thực hành update Stack và xử lý rollback khi template có lỗi, từ đó hiểu rõ vai trò quan trọng của Logs trong tab **Events**.

### 2. **Quản trị AWS bằng AWS CLI**

* Cài đặt và cấu hình AWS CLI v2, làm quen với dòng lệnh thông qua `aws configure` và quản lý profile nhiều môi trường.
* Thao tác với các dịch vụ AWS như **S3**, **EC2**, **IAM**, **CloudFormation** thông qua dòng lệnh.
* Viết script Bash nhỏ để tự động hoá việc tạo resource, từ đó giảm thao tác thủ công và tăng hiệu suất công việc.

---

## 📚 **Kiến thức và kỹ năng đạt được**

### 🔧 **Technical Skills**

* Viết CloudFormation Template bằng YAML.
* Sử dụng AWS CLI để tương tác với các dịch vụ: tạo bucket, launch EC2, tạo IAM user, deploy CloudFormation stack.
* Tư duy quản trị hạ tầng như quản lý mã nguồn: có version, rollback, debug.

### 💡 **Kiến thức lý thuyết**

* Hiểu rõ vai trò của **Infrastructure as Code** trong việc tự động hóa và kiểm soát hạ tầng.
* Nắm được kiến trúc hoạt động của CloudFormation và cơ chế rollback khi gặp lỗi.
* Nhận ra tầm quan trọng của **CLI** như là công cụ nền tảng cho DevOps và automation.

### 🤝 **Kỹ năng mềm phát triển**

* Kỷ luật trong việc viết YAML chính xác, chú ý đến indent và cấu trúc file.
* Tư duy debug có hệ thống (qua Logs, Events, CLI Output).
* Quản lý tài liệu lệnh, xây dựng “cheatsheet” riêng để dùng lại.

---

## 🚧 **Khó khăn & Cách khắc phục**

| Vấn đề                        | Nguyên nhân                            | Giải pháp                               | Kết quả & Bài học rút ra             |
| ----------------------------- | -------------------------------------- | --------------------------------------- | ------------------------------------ |
| YAML bị lỗi indentation       | Cú pháp YAML rất nhạy với khoảng trắng | Dùng VSCode + YAML Plugin để soạn       | Luôn validate YAML trước khi deploy  |
| Stack rollback không rõ lý do | Thiếu IAM Role hoặc config sai         | Đọc Events Logs để tìm nguyên nhân      | Log là chìa khóa để debug Cloud      |
| CLI không nhận Access Key     | Thiếu cấu hình `aws configure`         | Tạo `~/.aws/credentials` đúng định dạng | Luôn kiểm tra profile trước khi chạy |
| Khó nhớ cú pháp CLI phức tạp  | Lệnh dài, nhiều flag                   | Ghi lại lệnh mẫu, dùng script Bash      | Làm việc hiệu quả hơn, giảm sai sót  |

---

## 💭 **Reflection & Nhận xét cá nhân**

### ✅ **Những điều làm tốt**

* Tự tay viết và triển khai thành công CloudFormation Stack đầu tiên.
* Tự tin sử dụng dòng lệnh để thao tác với tài nguyên AWS.
* Nắm được tư duy “mã hoá hạ tầng” giúp tái sử dụng và dễ kiểm soát.

### 🔄 **Những điều cần cải thiện**

* Nên luyện tập nhiều hơn với CLI cho các dịch vụ như RDS, Lambda.
* Tập triển khai template từ CLI thay vì chỉ dùng Console.
* Kết hợp CloudFormation + CLI trong quy trình CI/CD nhỏ.

### 🌱 **Bài học sâu sắc**

> Việc sử dụng **Infrastructure as Code** và **CLI** không chỉ giúp tiết kiệm thời gian mà còn giúp mình kiểm soát hạ tầng tốt hơn, tự tin triển khai hệ thống theo cách tự động hóa, minh bạch và dễ debug. Đây chính là một bước chuyển mình quan trọng từ mindset "làm thủ công" sang "DevOps mindset".

Worklog created by: Tran Quang Trong