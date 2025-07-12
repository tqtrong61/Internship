# 📆 **TỔNG KẾT THÁNG 1 THỰC TẬP AWS (14/05 – 06/06/2025)**

**Tên thực tập sinh**: Trần Quang Trọng
**Thời gian thực tập**: Tuần 1 đến Tuần 4 / 12

---

## 🧭 **1. Hành trình học tập & Nội dung đã hoàn thành**

### 🔹 **Tuần 1 – Làm quen với AWS nền tảng**

* Hiểu về chi phí và cách quản lý ngân sách qua **AWS Budgets** và **Cost Explorer**.
* Tự tạo VPC với các thành phần mạng cơ bản: **Subnet**, **Route Table**, **NAT**, **IGW**.
* Khởi tạo và triển khai web đơn giản với **EC2**, **Elastic IP**, **Apache Web Server**.
* Kỹ năng đáng chú ý: mở port, cấu hình Security Group, gán quyền truy cập đúng cách.

### 🔹 **Tuần 2 – Hosting, DNS và Tự động hóa**

* Tạo website tĩnh với **S3 Static Hosting** và phân phối nội dung qua **CloudFront**.
* Quản lý domain với **Route 53** (health check, failover, bản ghi A/CNAME).
* Triển khai **Auto Scaling Group** và kiểm tra khả năng scale-in/scale-out thực tế.
* Tích hợp giám sát với **CloudWatch Metrics, Logs, Alarm + SNS**.
* Hiểu rõ mối liên kết giữa các thành phần frontend – backend – monitoring.

### 🔹 **Tuần 3 – Tự động hóa với IaC và AWS CLI**

* Viết và deploy **CloudFormation Template (YAML)** tạo S3, EC2, IAM Role…
* Sử dụng **AWS CLI** để tạo, kiểm tra và tự động hóa thao tác với AWS Services.
* Viết Bash Script nhỏ thao tác với CLI để tăng hiệu quả và tránh sai sót.
* Rút ra tư duy: **IaC là chìa khóa quản lý hạ tầng bền vững**, CLI là kỹ năng DevOps cơ bản.

### 🔹 **Tuần 4 – Kiến trúc sẵn sàng cao và Edge Computing**

* Xây dựng kiến trúc **2-tier (Web + DB)** dùng **Auto Scaling + ALB + RDS Multi-AZ**.
* Kiểm thử thực tế khả năng tự phục hồi của hệ thống khi lỗi EC2 hoặc failover DB.
* Triển khai **Lambda\@Edge** để xử lý logic redirect theo locale tại edge location.
* Hiểu được sức mạnh của **Edge Computing** trong tối ưu hóa trải nghiệm người dùng toàn cầu.

---

## 🧠 **2. Kiến thức lý thuyết nổi bật đã tiếp thu**

| Chủ đề                       | Nội dung chính                                          |
| ---------------------------- | ------------------------------------------------------- |
| ☁️ **AWS Core Services**     | EC2, S3, RDS, CloudFront, Route 53, CloudWatch          |
| 🛠️ **IaC & CLI**            | CloudFormation Template (YAML), AWS CLI v2              |
| 🔁 **Auto Scaling**          | ASG policies, Launch Template, test `stress-ng`         |
| 🌍 **Edge Computing**        | Lambda\@Edge, Viewer Request events, Region restriction |
| 📈 **Monitoring & Alerting** | CloudWatch Metrics, Logs, Alarm, SNS Email              |
| 🔐 **Networking & Security** | VPC, Subnet, NAT, Security Group, IAM Role/Policy       |
| 💸 **Cost Management**       | AWS Budgets, Alerts, Cost Explorer                      |

---

## 🔧 **3. Kỹ năng kỹ thuật đạt được**

| Kỹ năng                    | Mô tả                                                    |
| -------------------------- | -------------------------------------------------------- |
| 🚀 **Triển khai hệ thống** | Tự xây dựng kiến trúc HA – có Load Balancer, DB Multi-AZ |
| 🧱 **IaC thực chiến**      | Tạo stack CloudFormation cho S3, EC2, IAM từ YAML        |
| 💻 **CLI Automation**      | Dùng dòng lệnh để deploy dịch vụ, automate hạ tầng       |
| 🌐 **Quản lý mạng AWS**    | Hiểu cấu trúc mạng nội bộ và kết nối internet            |
| 📡 **Giám sát & phản ứng** | Tạo cảnh báo, log hệ thống, kiểm thử kịch bản lỗi        |

---

## ⚠️ **4. Những khó khăn đã gặp và cách khắc phục**

| Vấn đề                       | Nguyên nhân                      | Hướng giải quyết                              |
| ---------------------------- | -------------------------------- | --------------------------------------------- |
| EC2 không chạy web           | Script sai / chưa update OS      | Viết lại User Data có kiểm thử                |
| RDS không tạo được Multi-AZ  | Thiếu Subnet Group đủ AZ         | Tạo đủ 2 private subnet khác AZ               |
| Lambda\@Edge không hoạt động | Quên publish version             | Luôn **publish version mới** trước khi attach |
| Route 53 không resolve       | DNS chưa propagate / bản ghi sai | Kiểm tra TTL, bản ghi, dùng tool test         |
| Logs không hiển thị          | Agent thiếu quyền IAM            | Gán đúng policy `CloudWatchAgentServerPolicy` |

---

## 💬 **5. Reflection & Nhận xét cá nhân**

### ✅ **Những điểm nổi bật**

* Tự triển khai được hệ thống cloud end-to-end từ web tĩnh đến web động.
* Hiểu rõ mối quan hệ giữa kiến trúc mạng – compute – monitoring – cost – DNS.
* Tập luyện thói quen viết **Worklog chi tiết, có phân tích và mô hình hóa rõ ràng**.
* Có tư duy hệ thống và chịu trách nhiệm khi hệ thống gặp lỗi.

### 🔄 **Những điểm cần cải thiện**

* Cần luyện thêm kỹ năng debug logs từ CloudWatch và error từ CloudFormation Events.
* Nên tích hợp CloudFormation vào CI/CD pipeline sớm để thử nghiệm tự động hóa toàn bộ.
* Học sâu hơn về Lambda và Step Functions để xử lý các automation workflow.

---

## 🌟 **6. Bài học lớn nhất trong tháng này**

> Học AWS không chỉ là "biết dịch vụ nào để dùng", mà là **hiểu cách kết nối chúng thành một hệ thống sống** – có khả năng phục hồi, giám sát, tự động hóa và tối ưu chi phí.
> Những kỹ năng như **IaC, Auto Scaling, Edge Computing và Monitoring** sẽ là nền móng vững chắc cho giai đoạn tiếp theo: **CI/CD, Serverless Architecture, Hybrid Systems hoặc Multi-Region Deployment**.

Worklog created by: Tran Quang Trong