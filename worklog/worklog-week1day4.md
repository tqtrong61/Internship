# 📘 Worklog - Ngày 15/05/2025

## 📅 Thông tin cơ bản

- **Ngày**: 15/05/2025  
- **Thứ**: Thứ Năm  
- **Tuần thực tập**: Tuần thứ 1/12  
- **Thời gian làm việc**: 9:00 - 17:00  
- **Mood**: 🌐 + Hào hứng tìm hiểu mạng riêng ảo trên AWS

---

## 🎯 Mục tiêu ngày hôm nay

- Tìm hiểu khái niệm và chức năng của Amazon VPC
- Tạo một VPC tùy chỉnh với Subnet, Internet Gateway và Route Table
- Hiểu rõ vai trò của CIDR block và NAT Gateway trong kiến trúc VPC
- Kết nối EC2 vào VPC và truy cập internet

---

## 💼 Công việc đã thực hiện

### 1. Học khái niệm cơ bản về Amazon VPC ⏱️ 2 giờ

- **Mô tả**: Đọc tài liệu và video hướng dẫn trên AWS Study Group để hiểu VPC là gì
- **Kết quả**: Nắm được các thành phần chính như Subnet, IGW, Route Table, NAT, CIDR
- **Tools/Tech**: AWS Documentation, Study Group
- **Links**: [VPC Cơ bản](https://000003.awsstudygroup.com/vi/)

---

### 2. Tạo VPC tùy chỉnh qua VPC Wizard ⏱️ 2 giờ

- **Mô tả**: Dùng VPC Wizard để tạo 1 VPC có 2 Subnet (public & private), NAT Gateway và IGW
- **Kết quả**: Có thể truy cập EC2 ở public subnet từ internet, EC2 ở private subnet dùng NAT để ra ngoài
- **Tools/Tech**: AWS VPC Wizard, EC2
- **Links**: [Hướng dẫn tạo VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-wizard.html)

---

### 3. Tạo EC2 trong VPC và test kết nối internet ⏱️ 2 giờ

- **Mô tả**: Khởi tạo 2 EC2 instances, 1 trong public subnet, 1 trong private
- **Kết quả**: Instance public có thể SSH từ máy mình, instance private ping ra ngoài qua NAT Gateway
- **Tools/Tech**: EC2, SSH, Security Groups
- **Links**: [EC2 in VPC](https://docs.aws.amazon.com/vpc/latest/userguide/launch-instance.html)

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

- Tạo VPC tùy chỉnh với 2 loại Subnet: Public & Private
- Cấu hình Internet Gateway, Route Table, NAT Gateway
- Gán Security Group và kiểm tra kết nối EC2 qua SSH

### 💡 Concepts & Theory

- VPC là vùng mạng riêng ảo giúp cô lập tài nguyên AWS
- Public Subnet: Có route đến IGW → truy cập internet được
- Private Subnet: Không có route trực tiếp → cần NAT Gateway
- CIDR block xác định dải IP cho VPC và Subnet

### 🤝 Soft Skills

- Lập sơ đồ mạng trước khi cấu hình để dễ hình dung
- Cẩn thận khi chọn region, AZ để tránh lỗi tài nguyên không tương thích
- Kiên nhẫn khi gặp lỗi cấu hình mạng và security

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: EC2 private subnet không ra được internet

- **Mô tả**: EC2 trong private subnet không tải được gói update
- **Impact**: Không cài được phần mềm cần thiết
- **Root Cause**: Chưa gắn NAT Gateway hoặc cấu hình route sai
- **Solution**: Tạo NAT Gateway, gắn vào route table của private subnet
- **Result**: EC2 trong private subnet ra internet qua NAT bình thường
- **Lesson**: Route Table rất quan trọng, cần kiểm tra kỹ từng subnet

---

### Vấn đề 2: Không SSH được vào EC2 trong public subnet

- **Mô tả**: Dù gán Elastic IP nhưng không connect được
- **Impact**: Không test được connectivity
- **Root Cause**: Thiếu rule trong Security Group cho port 22
- **Solution**: Thêm inbound rule SSH (port 22) từ IP máy mình
- **Result**: SSH thành công
- **Lesson**: Security Group mặc định rất chặt → phải mở port thủ công

---

## 💭 Reflection & Insights

### ✅ What went well today?

- Tạo được VPC đầy đủ chức năng với 2 subnet
- Hiểu rõ cách EC2 kết nối internet qua IGW và NAT Gateway
- Tự fix được lỗi kết nối bằng cách xem route table và security group

### 🔄 What could be improved?

- Nên thử dùng VPC tự tạo thủ công thay vì dùng VPC Wizard
- Làm thử kiến trúc có nhiều AZ để tăng tính sẵn sàng
- Tìm hiểu thêm về VPC Peering và VPN Gateway

### 💡 Key Insights

- Hiểu VPC là bước đầu tiên để xây dựng kiến trúc mạng an toàn trên AWS
- Subnet + Route Table + IGW/NAT quyết định luồng truy cập trong VPC
- Mạng cloud không khó nếu có sơ đồ logic rõ ràng và làm từng bước chắc chắn

Worklog created by: Tran Quang Trong
