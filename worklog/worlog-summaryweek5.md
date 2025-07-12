# 📊 **Tổng Kết Tuần 5 Thực Tập (09/06 – 13/06/2025)**

**Thực tập sinh**: Trần Quang Trọng

---

## ✅ 1. **Những Kiến Thức và Kỹ Năng Đã Học Được**

### 🔧 **Technical Skills**

* Triển khai thành công **AWS Lambda** để tự động xử lý sự kiện từ **S3** và **CloudWatch Event**.
* Viết **Node.js Lambda function** tương tác với **S3 API** thông qua AWS SDK.
* Cấu hình **trigger** cho Lambda từ:

  * S3 (`ObjectCreated`)
  * EventBridge (`schedule mỗi 5–10 phút`)
* Thiết lập và test quy trình **Disaster Recovery** với **AWS Elastic Disaster Recovery (DRS)**:

  * Cài đặt Agent
  * Thiết lập replication
  * Thực hiện **Launch Test**
  * Mô phỏng **Failover & Recovery**
* Quản lý **IAM Roles** và **Policy** chính xác cho Lambda và DRS Agent.
* Debug qua **CloudWatch Logs**, kiểm tra event và xử lý lỗi phân quyền.

---

### 💡 **Kiến Thức Lý Thuyết**

* **Kiến trúc Serverless**: Không quản lý máy chủ, tự động scale, chỉ trả tiền theo thời gian chạy thực tế.
* **AWS Lambda** là dịch vụ cốt lõi của Serverless, hoạt động theo mô hình event-driven.
* **Disaster Recovery (DR)**:

  * Hiểu rõ khái niệm **RTO**, **RPO**
  * So sánh các mô hình DR: Backup & Restore, Pilot Light, Warm Standby, Multi-site
  * AWS DRS giúp giảm **RTO/RPO** mà không cần phần cứng on-premises
* **EventBridge (CloudWatch Events)**: Hỗ trợ trigger định kỳ hoặc theo thời gian biểu linh hoạt.
* Tự động hóa = tối ưu vận hành + giảm thiểu rủi ro thủ công.

---

### 🤝 **Soft Skills**

* Rèn luyện tư duy thiết kế kiến trúc **event-driven**.
* Ghi chú, lập quy trình và checklist rõ ràng khi test DR hoặc trigger Lambda.
* Kiên nhẫn và tỉ mỉ khi xử lý các lỗi nhỏ liên quan đến permission và hạ tầng.
* Học cách **debug theo hướng dẫn AWS Logs** và xác minh từng bước.

---

## 🚧 2. **Khó khăn đã gặp và cách khắc phục**

| Vấn đề                                  | Nguyên nhân                               | Giải pháp                                | Bài học                                          |
| --------------------------------------- | ----------------------------------------- | ---------------------------------------- | ------------------------------------------------ |
| Lambda không đọc được file S3           | Thiếu quyền `s3:GetObject`                | Gán IAM policy phù hợp                   | Cần hiểu rõ vai trò và quyền khi viết Lambda     |
| Lambda timeout                          | Xử lý file lớn, timeout mặc định quá ngắn | Tăng thời gian chạy lên 30s              | Tùy khối lượng xử lý mà cấu hình timeout phù hợp |
| CloudWatch Event không kích hoạt Lambda | Target ARN chưa đúng                      | Gắn lại ARN và xác minh qua log          | Phải kiểm tra toàn bộ pipeline event-trigger     |
| Agent DRS không kết nối được            | Security Group chặn outbound + sai IAM    | Mở port 443 + gán đúng role              | DRS phụ thuộc network và permission              |
| Test instance DRS lỗi volume            | Bản sao chưa sync xong                    | Chờ thêm thời gian + kiểm tra trạng thái | DR luôn cần buffer thời gian để đồng bộ ổn       |

---

## 💭 3. **Tổng kết và Bài học rút ra**

### ✅ **Điều làm tốt**

* Tự tay xây dựng luồng **Serverless Workflow** hoàn chỉnh.
* Triển khai thành công một mô hình DR thực tế.
* Hiểu sâu hơn về cách các dịch vụ AWS tích hợp và hoạt động cùng nhau.

### 🔄 **Cần cải thiện thêm**

* Làm quen với các công cụ **Infrastructure as Code** (SAM, CDK, Terraform).
* Thử nghiệm tích hợp Lambda với các dịch vụ khác như **API Gateway, DynamoDB**.
* Viết function bằng **Python** để linh hoạt hơn trong xử lý dữ liệu.
* Tự động hóa quy trình DR bằng script thay vì thao tác thủ công.

### 🌟 **Insights quan trọng**

* **Lambda + Event = Sức mạnh của Serverless**: giải quyết nhiều bài toán tự động hóa nhỏ, nhanh và rẻ.
* **Disaster Recovery không chỉ là backup** – mà là **khả năng phục hồi thật sự khi có sự cố**.
* **IAM Policy là xương sống của mọi dịch vụ AWS** – càng hiểu kỹ, triển khai càng mượt.
* **Tự động hóa workflow là cách để vận hành hệ thống linh hoạt, nhanh nhạy và ít sai sót hơn**.

Worklog created by: Tran Quang Trong