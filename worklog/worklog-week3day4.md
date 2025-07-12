# 📘 Worklog - Ngày 29/05/2025
## 📅 Thông tin cơ bản

* **Ngày**: 29/05/2025
* **Thứ**: Thứ Năm
* **Tuần thực tập**: Tuần thứ 3/12
* **Thời gian làm việc**: 9:00 - 17:00
* **Mood**: 💻 + Hứng thú vì cảm thấy mình như DevOps thực thụ khi dùng dòng lệnh

---

## 🎯 Mục tiêu ngày hôm nay

* Cài đặt và cấu hình AWS CLI v2
* Làm quen với các lệnh cơ bản: S3, EC2, IAM, CloudFormation
* Sử dụng CLI để tạo S3 bucket, launch EC2, và quản lý hạ tầng
* Tìm hiểu cách sử dụng AWS CLI với profile và region

---

## 💼 Công việc đã thực hiện

### 1. Cài đặt và cấu hình AWS CLI trên máy cá nhân ⏱️ 1 giờ

* **Mô tả**: Cài CLI v2 trên Windows, sử dụng `aws configure` để nhập Access Key
* **Kết quả**: CLI hoạt động ổn, kết nối đúng tài khoản AWS
* **Tools/Tech**: AWS CLI v2, IAM, Access Key
* **Links**: [Hướng dẫn CLI](https://000011.awsstudygroup.com/vi/)

---

### 2. Thao tác với S3 bằng CLI ⏱️ 1.5 giờ

* **Mô tả**: Tạo bucket, upload/download file, liệt kê object
* **Kết quả**: Quản lý hoàn toàn bucket mà không cần dùng Console
* **Lệnh tiêu biểu**:

  ```bash
  aws s3 mb s3://fcj-demo-cli-bucket
  aws s3 cp myfile.txt s3://fcj-demo-cli-bucket/
  aws s3 ls s3://fcj-demo-cli-bucket/
  ```

---

### 3. Tạo EC2 Instance bằng CLI ⏱️ 2 giờ

* **Mô tả**: Launch EC2 t2.micro từ AMI Amazon Linux 2, chỉ định key pair và security group
* **Kết quả**: EC2 khởi tạo thành công chỉ qua một lệnh duy nhất
* **Lệnh mẫu**:

  ```bash
  aws ec2 run-instances \
    --image-id ami-xxxxxxxxxxxxxxxxx \
    --count 1 \
    --instance-type t2.micro \
    --key-name my-key \
    --security-groups my-sg
  ```

---

### 4. Sử dụng CLI với IAM và CloudFormation ⏱️ 2 giờ

* **Mô tả**: Tạo user mới và gán policy bằng CLI, sau đó deploy CloudFormation stack từ file YAML
* **Kết quả**: Làm chủ các dịch vụ AWS mà không cần giao diện
* **Lệnh tiêu biểu**:

  ```bash
  aws iam create-user --user-name demo-cli-user
  aws cloudformation deploy \
    --template-file template.yml \
    --stack-name demo-stack \
    --capabilities CAPABILITY_NAMED_IAM
  ```

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

* Biết cách sử dụng `aws configure`, quản lý profile đa tài khoản
* Thao tác nhanh với các dịch vụ AWS bằng CLI
* Tạo tài nguyên tự động qua CLI thay vì click thủ công
* Hiểu cách kết hợp CLI + CloudFormation cho triển khai hạ tầng

### 💡 Concepts & Theory

* AWS CLI là công cụ command-line để quản trị tất cả dịch vụ AWS
* CLI có thể chạy lệnh đơn, script tự động, hoặc nhúng trong CI/CD
* Dùng profile để chia môi trường (dev, prod) mà không cần đổi account

### 🤝 Soft Skills

* Thành thạo thao tác với Terminal và cú pháp Bash
* Quản lý Access Key cẩn thận để tránh rò rỉ thông tin
* Biết debug lỗi từ CLI output và tìm log tương ứng

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Lỗi “Unable to locate credentials”

* **Mô tả**: CLI không nhận được Access Key để thực hiện lệnh
* **Impact**: Không thao tác được với AWS
* **Root Cause**: Chưa cấu hình `aws configure` hoặc thiếu profile
* **Solution**: Chạy lại `aws configure` và tạo file `~/.aws/credentials`
* **Result**: CLI hoạt động bình thường
* **Lesson**: Luôn xác minh AWS profile đang dùng qua `aws configure list`

---

### Vấn đề 2: Không nhớ được cú pháp lệnh dài

* **Mô tả**: Khó nhớ các flags và syntax phức tạp
* **Impact**: Tốn thời gian gõ sai lệnh nhiều lần
* **Solution**: Tạo file script `.sh` và dùng lệnh `aws help`, `--generate-cli-skeleton`
* **Result**: Giảm sai sót, làm việc nhanh hơn
* **Lesson**: Nên dùng alias, shell script hoặc lệnh mẫu copy sẵn

---

## 💭 Reflection & Insights

### ✅ What went well today?

* Làm chủ được thao tác cơ bản của AWS CLI với S3 và EC2
* Thấy rõ sự khác biệt giữa thao tác Console và CLI
* Viết được script nhỏ để tự động hoá việc deploy

### 🔄 What could be improved?

* Nên tập luyện thêm với dịch vụ RDS, Lambda qua CLI
* Tìm hiểu thêm CLI v2 với `aws sso login` thay vì dùng Access Key
* Ghi chú lệnh hay dùng thành một “cheatsheet” cá nhân

### 💡 Key Insights

* AWS CLI là nền tảng cho automation, scripting và DevOps
* Có thể quản lý toàn bộ cloud infrastructure chỉ bằng terminal
* CLI giúp tăng hiệu suất, tái sử dụng và kiểm soát môi trường tốt hơn GUI

Worklog created by: Tran Quang Trong