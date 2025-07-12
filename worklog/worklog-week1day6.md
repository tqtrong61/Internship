# 📘 Worklog - Ngày 17/05/2025

## 📅 Thông tin cơ bản

- **Ngày**: 17/05/2025  
- **Thứ**: Thứ Bảy  
- **Tuần thực tập**: Tuần thứ 1/12  
- **Thời gian làm việc**: 9:00 - 17:00  
- **Mood**: ⚙️ + Phấn khích khi tạo máy chủ ảo đầu tiên với EC2

---

## 🎯 Mục tiêu ngày hôm nay

- Làm quen với Amazon EC2 và các tùy chọn cấu hình khi khởi tạo
- Tạo EC2 instance (Ubuntu) và kết nối qua SSH
- Hiểu cách Elastic IP, Security Group, và Key Pair hoạt động
- Triển khai thử web server Apache lên EC2

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu tổng quan về Amazon EC2 ⏱️ 2 giờ

- **Mô tả**: Đọc tài liệu hướng dẫn từ Study Group để nắm khái niệm cơ bản về EC2
- **Kết quả**: Hiểu rõ EC2 là dịch vụ cung cấp máy chủ ảo theo yêu cầu (IaaS)
- **Tools/Tech**: AWS Console, EC2 Dashboard
- **Links**: [EC2 Overview](https://000004.awsstudygroup.com/vi/)

---

### 2. Khởi tạo EC2 instance và kết nối SSH ⏱️ 2 giờ

- **Mô tả**: Tạo 1 instance Ubuntu t2.micro (Free Tier), gán Elastic IP, tạo key pair
- **Kết quả**: Kết nối thành công từ terminal bằng SSH key
- **Tools/Tech**: EC2, SSH, Elastic IP, Security Group
- **Links**: [Khởi tạo EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)

---

### 3. Cài đặt Web Server trên EC2 ⏱️ 2 giờ

- **Mô tả**: Cài Apache HTTP Server và tạo trang index.html đơn giản
- **Kết quả**: Truy cập địa chỉ Elastic IP thấy trang "Hello from EC2!"
- **Tools/Tech**: Apache2, Ubuntu CLI
- **Links**: [Cài Apache](https://ubuntu.com/server/docs/web-servers-apache)

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

- Tạo EC2 với cấu hình Free Tier phù hợp cho người mới
- Tạo Key Pair và SSH vào instance qua terminal
- Gán Elastic IP và mở cổng HTTP (port 80) trên Security Group

### 💡 Concepts & Theory

- EC2 là dịch vụ IaaS giúp chạy ứng dụng trên máy ảo linh hoạt
- Elastic IP giữ địa chỉ IP tĩnh cho EC2 dù restart
- Security Group hoạt động như firewall cho instance
- Key Pair dùng để bảo mật truy cập bằng SSH thay vì username/password

### 🤝 Soft Skills

- Rèn luyện cách đọc log khi không kết nối được SSH
- Tự tổ chức command-line workflow rõ ràng: tạo key, chmod, ssh
- Kỹ năng troubleshoot kết nối mạng và quyền truy cập

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Không SSH được vào EC2

- **Mô tả**: EC2 đã chạy nhưng kết nối SSH bị timeout
- **Impact**: Không thao tác được bên trong instance
- **Root Cause**: Quên mở port 22 trong Security Group
- **Solution**: Thêm rule "SSH from My IP" vào Security Group
- **Result**: Kết nối thành công
- **Lesson**: Cần kiểm tra kỹ cấu hình mạng trước khi SSH

---

### Vấn đề 2: Không hiển thị trang web sau khi cài Apache

- **Mô tả**: Truy cập IP không thấy web chạy
- **Impact**: Không xác minh được thành công cài đặt Apache
- **Root Cause**: Port 80 chưa mở hoặc Apache chưa start
- **Solution**: Mở HTTP (port 80) và khởi động lại Apache
- **Result**: Website hoạt động bình thường
- **Lesson**: Luôn kiểm tra cả firewall (SG) và trạng thái dịch vụ

---

## 💭 Reflection & Insights

### ✅ What went well today?

- Lần đầu khởi tạo EC2 thành công và truy cập được qua SSH
- Cài đặt web server đơn giản để test public traffic
- Hiểu rõ mối liên hệ giữa Elastic IP, SG, và EC2 instance

### 🔄 What could be improved?

- Nên tạo script user-data để tự động cài Apache khi khởi tạo
- Thử launch EC2 bằng AWS CLI thay vì qua Console
- Thử cấu hình load balancer hoặc auto scaling group

### 💡 Key Insights

- EC2 là dịch vụ cốt lõi, cần hiểu kỹ vì rất thường xuyên dùng
- Quản lý Security Group & Key Pair tốt giúp giảm lỗi kết nối
- Việc thực hành hands-on giúp hiểu AWS nhanh hơn nhiều lý thuyết

Worklog created by: Tran Quang Trong
