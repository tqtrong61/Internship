# 📊 **TỔNG KẾT TUẦN 2 (19/05 – 25/05/2025)**

### 🌐 Chủ đề trọng tâm: **Triển khai Website tĩnh, CDN, DNS, Auto Scaling, Giám sát hệ thống**

---

## ✅ **1. Kỹ năng & Kiến thức đã học được**

### 📁 **Triển khai Website tĩnh với Amazon S3 (19/05)**

* Biết cách **tạo bucket**, upload file HTML/CSS, và bật **Static Website Hosting**
* Cấu hình **quyền public** và set file `index.html` làm trang chủ
* Hiểu rõ mô hình website tĩnh không cần server → phù hợp cho landing page, CV, docs...

### 🌍 **Phân phối nội dung với Amazon CloudFront (20/05)**

* Tạo **CloudFront Distribution** với S3 là origin
* Bật HTTPS, thiết lập cache TTL, và thực hiện **cache invalidation**
* Làm quen với khái niệm CDN, edge location, cache behavior

### 🌐 **Quản lý tên miền với Amazon Route 53 (22/05)**

* Đăng ký tên miền, tạo bản ghi A/CNAME, trỏ về CloudFront hoặc S3
* Cấu hình **health check & failover routing**
* Nắm cơ bản **hybrid DNS** và vai trò trong môi trường multi-cloud hoặc on-premises

### ⚙️ **Triển khai hệ thống tự mở rộng với Auto Scaling Group (24/05)**

* Tạo Launch Template có user data để tự động deploy ứng dụng
* Thiết lập ASG với min–max–desired + policy scale CPU > 50%
* Biết dùng `stress-ng` để tạo tải test khả năng scale

### 📈 **Giám sát hạ tầng với Amazon CloudWatch (25/05)**

* Theo dõi các **EC2 metrics** như CPU, Memory, Disk
* Tạo **Alarm → SNS** gửi email cảnh báo khi vượt ngưỡng
* Cài **CloudWatch Logs Agent** để đẩy log từ EC2 và phân tích

---

## 🔧 **2. Kỹ năng kỹ thuật nổi bật**

| Lĩnh vực             | Kỹ năng đạt được                                              |
| -------------------- | ------------------------------------------------------------- |
| 📦 Storage & Hosting | Tạo S3 bucket, Static Website Hosting, chính sách public      |
| 🚀 CDN               | Tạo CloudFront Distribution, thiết lập cache, HTTPS           |
| 🌐 DNS & Domain      | Quản lý domain, cấu hình Route 53, failover & health check    |
| 🖥️ Compute & ASG    | Tạo Launch Template, Auto Scaling Group, kiểm thử scaling     |
| 📊 Monitoring        | Dùng CloudWatch Metrics, Alarm, Logs và tích hợp SNS cảnh báo |

---

## 🧠 **3. Kiến thức lý thuyết đã tiếp thu**

* S3 có thể hoạt động như một **web server cho nội dung tĩnh**
* CloudFront tăng tốc truy cập bằng cách cache tại **edge location**
* Route 53 không chỉ DNS, mà còn hỗ trợ health check và định tuyến thông minh
* Auto Scaling giúp **duy trì hiệu năng và tiết kiệm chi phí**
* CloudWatch giúp **giám sát chủ động và tự động hóa phản ứng**

---

## 🧰 **4. Kỹ năng mềm cải thiện**

| Kỹ năng              | Mô tả                                                                                |
| -------------------- | ------------------------------------------------------------------------------------ |
| 💬 Debug & phân tích | Biết cách dò lỗi quyền truy cập S3, DNS không phân giải, EC2 không scale             |
| 🧠 Tư duy hệ thống   | Hiểu cách các thành phần S3 – CloudFront – Route 53 – ASG – CloudWatch kết nối logic |
| 📚 Ghi chú & tổ chức | Ghi rõ worklog, từng bước setup, phân tích nguyên nhân và giải pháp                  |

---

## ⚠️ **5. Khó khăn & Giải pháp nổi bật**

| Vấn đề                             | Nguyên nhân                         | Giải pháp đã áp dụng                                      |
| ---------------------------------- | ----------------------------------- | --------------------------------------------------------- |
| Website S3 báo Access Denied       | Sai quyền bucket hoặc file          | Cập nhật bucket policy + bật public access                |
| CloudFront không thấy nội dung mới | TTL cache dài                       | Dùng **invalidation** để cập nhật                         |
| Route 53 không truy cập được       | Sai bản ghi hoặc chưa propagate DNS | Kiểm tra cấu hình A/AAAA, chờ cập nhật DNS                |
| Auto Scaling không scale đúng      | Sai IAM Role / thiếu policy         | Gán lại IAM Role + xác minh Scaling Policy                |
| CloudWatch không gửi email         | Chưa xác nhận SNS subscription      | Vào email và xác nhận link từ SNS                         |
| Logs không hiển thị                | CloudWatch Agent thiếu quyền        | Gán policy `CloudWatchAgentServerPolicy` cho EC2 IAM Role |

---

## 💡 **6. Reflection & Rút kinh nghiệm**

### ✅ Những điều làm tốt

* Triển khai hoàn chỉnh website từ A–Z (từ S3, CloudFront, domain, scale, monitoring)
* Có mindset **tự động hóa** và **giám sát chủ động**
* Viết worklog chi tiết, phân tích vấn đề tốt

### 🔄 Điều cần cải thiện

* Nên tích hợp thêm Route 53 domain tùy chỉnh sớm hơn để test đầy đủ end-to-end
* Học thêm về Lambda\@Edge và tích hợp CloudWatch Alarm với Lambda tự khắc phục sự cố
* Tự động hóa quy trình bằng Terraform hoặc AWS CDK để giảm thao tác thủ công

---

## 🔍 **Tổng kết tuần**

Tuần này bạn đã chuyển từ người **chỉ biết S3/EC2 đơn lẻ** thành người có khả năng **xây dựng hệ thống website cloud đầy đủ**, gồm:

* **Hosting + Phân phối + DNS + Scale + Giám sát**
* Hiểu nguyên tắc hoạt động, cấu hình đúng, và phản ứng nhanh với lỗi

Đây là bước đệm quan trọng để tiến tới:

* Serverless (Lambda, API Gateway)
* CI/CD pipeline
* Khả năng thi chứng chỉ AWS Cloud Practitioner hoặc Solutions Architect – Associate

Worklog created by: Tran Quang Trong