# 📘 Worklog - Ngày 20/05/2025

## 📅 Thông tin cơ bản

- **Ngày**: 20/05/2025  
- **Thứ**: Thứ Ba  
- **Tuần thực tập**: Tuần thứ 2/12  
- **Thời gian làm việc**: 9:00 - 17:00  
- **Mood**: ⚡ + Hứng khởi khi tăng tốc độ truy cập website với CloudFront

---

## 🎯 Mục tiêu ngày hôm nay

- Tìm hiểu cách Amazon CloudFront hoạt động như CDN
- Tạo CloudFront Distribution để phục vụ nội dung từ S3
- Cấu hình cache behavior và SSL
- Kiểm tra tốc độ tải nội dung qua CloudFront so với S3 gốc

---

## 💼 Công việc đã thực hiện

### 1. Tìm hiểu khái niệm CDN và CloudFront ⏱️ 1.5 giờ

- **Mô tả**: Nắm được vai trò của CloudFront trong việc tối ưu tải nội dung tĩnh/dynamic
- **Kết quả**: Hiểu nguyên lý edge location, origin, caching, TTL
- **Tools/Tech**: Amazon CloudFront, Study Group Docs
- **Links**: [CloudFront Overview](https://000094.awsstudygroup.com/vi/)

---

### 2. Tạo CloudFront Distribution trỏ tới S3 website ⏱️ 2 giờ

- **Mô tả**: Trỏ origin là static S3 website đã tạo trước đó, bật redirect HTTP → HTTPS
- **Kết quả**: CloudFront phân phối được nội dung từ S3 nhanh hơn và an toàn hơn
- **Tools/Tech**: CloudFront, S3, SSL/TLS
- **Links**: [Create Distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-creating-console.html)

---

### 3. Tùy chỉnh Cache Behavior và thử nghiệm thực tế ⏱️ 2 giờ

- **Mô tả**: Điều chỉnh TTL và test thử khi cập nhật nội dung index.html
- **Kết quả**: Hiểu cache hoạt động ra sao và cách invalidation
- **Tools/Tech**: Cache Policy, Invalidations
- **Links**: [Caching in CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html)

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

- Tạo CloudFront Distribution từ S3 origin
- Bật redirect HTTPS và gắn SSL mặc định
- Quản lý cache bằng TTL và invalidation

### 💡 Concepts & Theory

- CloudFront là CDN giúp tăng tốc độ phân phối nội dung bằng cách đặt edge server gần người dùng
- Origin có thể là S3, EC2, ALB hoặc custom domain
- Cache behavior xác định cách CloudFront lưu và làm mới nội dung

### 🤝 Soft Skills

- Kỹ năng thử nghiệm A/B trước và sau khi dùng CloudFront
- Kiên nhẫn chờ phân phối DNS và invalidate cache đúng cách
- Biết đánh giá hiệu năng bằng trình duyệt và devtools

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Không thấy update nội dung khi sửa HTML

- **Mô tả**: Đã sửa index.html nhưng CloudFront vẫn hiển thị phiên bản cũ
- **Impact**: Người dùng không thấy nội dung mới
- **Root Cause**: CloudFront vẫn cache nội dung cũ do TTL dài
- **Solution**: Dùng chức năng invalidation để xóa cache cũ
- **Result**: Trang web hiển thị đúng nội dung mới
- **Lesson**: Cần lên chiến lược cache phù hợp với nội dung

---

### Vấn đề 2: CloudFront không phân phối được nội dung từ S3

- **Mô tả**: Lỗi Access Denied khi truy cập link CloudFront
- **Impact**: Website không tải được
- **Root Cause**: S3 không cho phép public hoặc không có OAI (Origin Access Identity)
- **Solution**: Cấu hình OAI cho CloudFront và cấp quyền truy cập S3 qua policy
- **Result**: Nội dung phân phối mượt mà và bảo mật hơn
- **Lesson**: Dùng OAI giúp bảo mật hơn so với public access

---

## 💭 Reflection & Insights

### ✅ What went well today?

- Tạo được CDN chuyên nghiệp phân phối website nhanh và bảo mật
- Hiểu được cách hoạt động của hệ thống caching và các edge location
- Tối ưu trang web tĩnh bằng cách kết hợp S3 và CloudFront

### 🔄 What could be improved?

- Nên thử thêm tích hợp Route 53 với domain tuỳ chỉnh
- Cấu hình thêm các cache policy riêng biệt cho từng loại file (HTML vs CSS)
- Tìm hiểu về Lambda@Edge để xử lý nội dung tại edge location

### 💡 Key Insights

- CloudFront giúp cải thiện tốc độ và độ ổn định cho bất kỳ website nào
- Invalidating cache là kỹ năng quan trọng khi deploy nội dung mới
- Cấu hình đúng cache và origin giúp tiết kiệm chi phí và nâng cao trải nghiệm người dùng

Worklog created by: Tran Quang Trong


