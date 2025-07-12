# 📊 **Tổng kết Tuần 4 Thực Tập (Tuần từ 02/06/2025 - 06/06/2025)**

**Tên thực tập sinh**: Trần Quang Trọng
**Thời gian thực tập**: Tuần thứ 4/12

---

## 🎯 **Tổng quan nội dung học trong tuần**

### 1. **Xây dựng Hệ thống Ứng dụng Web có Tính Sẵn Sàng Cao (High Availability Web Architecture)**

* Thiết lập kiến trúc 2-tier gồm Web Layer và Database Layer sử dụng các dịch vụ:

  * **Auto Scaling Group (ASG)** và **Application Load Balancer (ALB)** để xử lý phần Web.
  * **Amazon RDS Multi-AZ** để đảm bảo tính liên tục của cơ sở dữ liệu.
* Cấu hình mạng: VPC, subnet công khai và riêng tư, route table, NAT Gateway để đảm bảo tính bảo mật và phân tách rõ ràng các tầng ứng dụng.
* Kiểm thử các tính năng tự phục hồi như:

  * ASG tự tạo lại EC2 khi bị lỗi.
  * RDS tự động failover khi AZ chính gặp sự cố.

### 2. **Khám phá Lambda\@Edge và CloudFront CDN**

* Cấu hình **Amazon CloudFront** phân phối website tĩnh từ **S3 Static Website**.
* Viết và triển khai hàm **Lambda\@Edge** để xử lý logic chuyển hướng dựa trên `Accept-Language` của trình duyệt.
* Hiểu được cơ chế hoạt động của các **edge location** và cách Lambda được thực thi tại đó.
* Kiểm tra hoạt động thực tế của Lambda function khi người dùng truy cập từ vị trí địa lý khác nhau.

---

## 📚 **Kiến thức và kỹ năng đạt được**

### 🔧 **Technical Skills**

* Triển khai hệ thống web nhiều tầng có khả năng tự mở rộng và chịu lỗi.
* Cấu hình RDS dạng Multi-AZ để đảm bảo uptime cao.
* Cấu hình và sử dụng CloudFront để tăng tốc độ phân phối nội dung.
* Viết Lambda function xử lý các sự kiện edge như Viewer Request.
* Gắn Lambda\@Edge với CloudFront và kiểm tra propagation trên edge.

### 💡 **Kiến thức lý thuyết**

* **High Availability**: Hệ thống có khả năng tiếp tục hoạt động khi một phần bị lỗi.
* **Fault Tolerance**: Khả năng chịu lỗi mà không làm gián đoạn dịch vụ.
* **Edge Computing**: Xử lý yêu cầu tại vị trí gần người dùng nhất để giảm độ trễ.
* **Lambda\@Edge**: Serverless function chạy ở edge location, hỗ trợ personalization và bảo mật sớm (early request filtering).

### 🤝 **Kỹ năng mềm**

* Tư duy thiết kế hệ thống chịu lỗi và sẵn sàng cao như môi trường thực tế.
* Ghi chú kỹ càng từng thành phần kiến trúc để dễ bảo trì và triển khai lại.
* Debug các tình huống thực tế như script lỗi, failover DB, hoặc propagation delay.

---

## 🚧 **Khó khăn & Bài học rút ra**

| Vấn đề                       | Nguyên nhân                                         | Cách khắc phục                                | Bài học rút ra                                          |
| ---------------------------- | --------------------------------------------------- | --------------------------------------------- | ------------------------------------------------------- |
| EC2 không chạy Apache        | Script User Data sai quyền hoặc thiếu `#!/bin/bash` | Sửa lại script đúng cú pháp và đầy đủ command | Cẩn thận validate script User Data trước khi dùng       |
| RDS Multi-AZ lỗi             | Subnet Group chưa đầy đủ AZ                         | Tạo đủ Subnet ở nhiều AZ và gán đúng          | AWS yêu cầu nghiêm ngặt về vùng và subnet               |
| Lambda\@Edge không hoạt động | Chưa publish version trước khi attach               | Publish version mới trước khi liên kết        | Lambda\@Edge cần phiên bản cụ thể, không dùng bản draft |
| Lambda deploy sai region     | Không deploy ở `us-east-1`                          | Triển khai lại đúng vùng Virginia             | Lambda\@Edge chỉ hỗ trợ từ US East (N. Virginia)        |

---

## 💭 **Reflection & Nhận xét cá nhân**

### ✅ **Những điều làm tốt**

* Hoàn thiện thành công hệ thống kiến trúc HA có thể triển khai trong thực tế.
* Hiểu rõ mối quan hệ giữa các dịch vụ AWS khi xây dựng hạ tầng có tính sẵn sàng cao.
* Thực hành được mô hình **edge computing** với Lambda\@Edge và CloudFront.

### 🔄 **Những điều có thể cải thiện**

* Cần ghi log đầy đủ vào **CloudWatch** để theo dõi hệ thống tốt hơn.
* Có thể tự động hóa toàn bộ quá trình với **CloudFormation hoặc Terraform**.
* Nên thử nghiệm trong nhiều tình huống khác như thừa tải hoặc ngắt hoàn toàn 1 AZ.

### 🌟 **Key Insights**

> Một hệ thống **High Availability** không chỉ là việc chạy nhiều máy, mà là sự phối hợp nhịp nhàng của các thành phần tự động mở rộng, cân bằng tải, phát hiện lỗi và phục hồi.
> Lambda\@Edge là minh chứng rõ ràng cho thấy **việc xử lý logic gần người dùng nhất** có thể giảm tải backend và cải thiện trải nghiệm người dùng đáng kể.

Worklog created by: Tran Quang Trong