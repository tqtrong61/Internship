# 📊 **TỔNG KẾT TUẦN 1 (14/05 – 17/05/2025)**

### 🎯 Chủ đề chính: Làm quen với AWS cơ bản – Chi phí, mạng VPC, và EC2

---

## ✅ **Những gì đã học được**

### 1. **Quản lý chi phí với AWS Budgets (14/05)**

* Biết cách **tạo ngân sách**, thiết lập cảnh báo qua **SNS**, và phân tích chi phí bằng **Cost Explorer**.
* Nhận ra EC2 là dịch vụ tốn chi phí nhiều nhất nếu không tắt đúng cách.
* Bài học: **Theo dõi ngân sách liên tục** và cần xác nhận email để nhận alert từ SNS.

### 2. **Tạo và cấu hình mạng riêng ảo với VPC (15/05)**

* Tự tay tạo VPC với **2 subnet** (public + private), cấu hình **IGW**, **NAT Gateway**, và **Route Table**.
* Hiểu mối liên hệ giữa subnet, route và quyền kết nối ra internet.
* Fix lỗi không SSH được và EC2 không truy cập ra ngoài bằng cách xử lý cấu hình mạng và SG.

### 3. **Khởi tạo và triển khai ứng dụng đơn giản với EC2 (17/05)**

* Tạo EC2 Ubuntu, SSH vào bằng key pair, cài **Apache web server** và gán Elastic IP để public.
* Nắm vững mối liên hệ giữa **Elastic IP – Security Group – EC2 instance**.
* Fix lỗi SSH và HTTP bằng cách kiểm tra port (22 và 80) và dịch vụ chạy trong EC2.

---

## 🔧 **Kỹ năng kỹ thuật đạt được**

| Mảng kỹ năng | Nội dung                                         |
| ------------ | ------------------------------------------------ |
| ☁️ AWS Cost  | Tạo budget, cảnh báo chi phí, dùng Cost Explorer |
| 🌐 VPC       | Tạo subnet, NAT, IGW, cấu hình route table       |
| 🖥️ EC2      | Tạo instance, SSH, gán Elastic IP, cài Apache    |
| 🔒 Security  | Quản lý Security Group, hiểu role của firewall   |

---

## 💡 **Insights & Tư duy rút ra**

* AWS cung cấp nhiều công cụ hỗ trợ người mới **kiểm soát chi phí** từ sớm.
* Hiểu đúng về **mạng và kết nối** giúp giải quyết 80% vấn đề khi khởi tạo hệ thống.
* **Thực hành trực tiếp** mang lại hiệu quả học nhanh hơn đọc lý thuyết.
* Những lỗi nhỏ như quên mở port hoặc quên assign IAM Role có thể khiến hệ thống không chạy được → cần **check kỹ từng bước**.

---

## ⚠️ **Khó khăn & Kinh nghiệm**

| Vấn đề                       | Nguyên nhân                      | Kinh nghiệm rút ra                         |
| ---------------------------- | -------------------------------- | ------------------------------------------ |
| Không nhận được cảnh báo SNS | Chưa xác nhận subscription email | Luôn kiểm tra inbox khi setup SNS          |
| EC2 không ra internet        | Thiếu NAT Gateway / route table  | Route table cần thiết cho mỗi subnet       |
| SSH không được               | Chưa mở port 22                  | Kiểm tra Security Group ngay khi lỗi SSH   |
| Web không chạy               | Apache chưa start / SG chưa mở   | Kiểm tra cả trạng thái dịch vụ và firewall |

---

## 🪞 **Reflection tổng thể**

* Đây là tuần **làm quen nền tảng hạ tầng cơ bản nhất của AWS**, gồm chi phí, mạng, và compute.
* Mỗi ngày đều có **thử – sai – sửa – học**, đúng tinh thần thực tập.
* Các kỹ năng đạt được là nền móng để học tiếp các phần cao hơn như Auto Scaling, Load Balancing, Serverless.

Worklog created by: Tran Quang Trong
