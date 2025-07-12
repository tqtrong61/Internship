## 📄 1. Executive Summary (10%)
Trong thời đại số, dữ liệu sao lưu (backup) và lưu trữ dài hạn (archive) ngày càng chiếm phần lớn trong tổng chi phí vận hành của một hệ thống CNTT. Với việc phần lớn dữ liệu chỉ được truy cập rất ít hoặc không bao giờ truy cập lại, việc lưu trữ tất cả trong một lớp lưu trữ tiêu chuẩn (Standard) là cực kỳ tốn kém và không hiệu quả.

Đề tài này nhằm giải quyết bài toán lưu trữ backup hiệu quả thông qua việc sử dụng dịch vụ **Amazon S3 Intelligent-Tiering**, kết hợp với các tính năng như **Object Lock**, **Glacier**, và **giám sát chi phí** bằng các công cụ AWS như **CloudWatch**, **Cost Explorer**, **S3 Analytics**.

Người thực hiện sẽ tiến hành thử nghiệm triển khai toàn bộ hệ thống backup đơn giản trong môi trường mô phỏng trên tài khoản AWS cá nhân (miễn phí trong 30 ngày đầu với dung lượng nhỏ). Đề tài không yêu cầu dữ liệu thực tế hay môi trường sản xuất, giúp người học tiếp cận nền tảng AWS từng bước một cách rõ ràng và dễ hiểu.

### 🔑 Tóm tắt các điểm nổi bật:

| Mục                    | Nội dung                                                                 |
| ---------------------- | ------------------------------------------------------------------------ |
| 🎯 Vấn đề              | Lưu trữ sao lưu tốn kém, thiếu phân tầng, không có bảo vệ dữ liệu        |
| 💡 Giải pháp           | S3 Intelligent-Tiering, Object Lock, Glacier, giám sát & tự động hóa     |
| 📈 Lợi ích             | Giảm 60–70% chi phí lưu trữ, bảo vệ dữ liệu, nâng cao kỹ năng triển khai |
| ⏳ Thời gian triển khai | 20 ngày, triển khai theo từng giai đoạn nhỏ                              |
| 💰 Ngân sách           | Gần như miễn phí nhờ Free Tier AWS                                       |
| ✅ Kết quả mong đợi     | Hiểu cơ chế phân tầng, tự động hóa backup, kiểm tra bảo vệ & phục hồi    |

Dự án được đánh giá theo các tiêu chí:

* Ngắn gọn, rõ ràng, tập trung vào giải pháp
* Tính khả thi cao trong thời gian ngắn
* Tăng cường kỹ năng thực hành AWS cho người mới

---

## 🎯 2. Problem Statement (15%)

### 🧩 Current Situation

Trong nhiều hệ thống, việc lưu trữ backup thường được thực hiện theo cách truyền thống – tất cả dữ liệu được đưa vào một lớp lưu trữ duy nhất, thường là “Standard”. Việc này dẫn đến chi phí duy trì cực kỳ cao, đặc biệt khi dung lượng tăng theo thời gian.

### ❗ Key Challenges

1. **Không phân biệt dữ liệu truy cập thường xuyên và ít truy cập:**
   → Mọi thứ đều nằm trong cùng một lớp đắt tiền.

2. **Không có bảo vệ khỏi thao tác xóa nhầm:**
   → Nếu người dùng vô tình xóa bản sao lưu, không thể phục hồi.

3. **Không có theo dõi phân tích hiệu quả sử dụng:**
   → Không biết file nào đang “chiếm chỗ” lâu dài mà không có giá trị.

4. **Chi phí tăng không kiểm soát theo thời gian:**
   → Gây ảnh hưởng đến ngân sách vận hành.

### 👥 Stakeholder Impact

| Đối tượng                       | Ảnh hưởng                                 |
| ------------------------------- | ----------------------------------------- |
| Quản trị viên hệ thống (SysOps) | Mất công giám sát thủ công, dễ sai sót    |
| Người dùng cuối                 | Dữ liệu backup có thể không phục hồi được |
| Người lập kế hoạch ngân sách    | Chi phí lưu trữ tăng không rõ nguyên nhân |

### 💣 Business Consequences

* Rủi ro mất dữ liệu nếu backup bị ghi đè hoặc xóa nhầm.
* Lãng phí chi phí vì lưu trữ không hiệu quả.
* Không đạt yêu cầu tuân thủ (compliance) về thời gian lưu giữ.

### 🌐 Market Opportunity

Giải pháp đơn giản này phù hợp với:

* Các tổ chức nhỏ hoặc nhóm sinh viên thực tập cần hệ thống backup.
* Người mới bắt đầu làm quen với AWS mà chưa có kinh nghiệm sản xuất.
* Các công ty SME muốn lưu trữ backup dài hạn với chi phí thấp.

## 🏗️ 3. Solution Architecture (25%)

### 🗺️ Architecture Overview

Kiến trúc giải pháp được xây dựng xoay quanh **Amazon S3**, tận dụng các tính năng lưu trữ phân tầng tự động (Intelligent-Tiering), bảo vệ dữ liệu (Object Lock), và lưu trữ lạnh chi phí thấp (S3 Glacier). Tất cả được triển khai và vận hành trên tài khoản cá nhân của người học để đảm bảo đơn giản, tiết kiệm và dễ kiểm soát.

Dưới đây là sơ đồ kiến trúc cấp cao:

```
+-------------------------+
|   User / Admin Uploads |
+-------------------------+
            |
            v
+-----------------------------+
|      Amazon S3 Bucket       |
| - Intelligent-Tiering       |
| - Object Lock (Governance)  |
+-----------------------------+
            |
            v
+-----------------------------+
|   Automatic Tier Transitions|
| - Frequent -> Infrequent    |
| - Infrequent -> Glacier     |
+-----------------------------+
            |
            v
+-----------------------------+
|   Monitoring & Cost Control |
| - CloudWatch Logs           |
| - AWS Config & S3 Analytics |
| - Cost Explorer             |
+-----------------------------+
```

---

### 🛠️ AWS Services Used

| Dịch vụ                                  | Vai trò trong giải pháp                        |
| ---------------------------------------- | ---------------------------------------------- |
| **Amazon S3**                            | Lưu trữ chính các file backup                  |
| **S3 Intelligent-Tiering**               | Tự động di chuyển dữ liệu theo mức độ truy cập |
| **S3 Object Lock**                       | Ngăn không cho xóa nhầm dữ liệu                |
| **S3 Glacier / Glacier Deep Archive**    | Lưu trữ dữ liệu lâu dài với chi phí rất thấp   |
| **AWS CloudWatch**                       | Theo dõi truy cập và cảnh báo                  |
| **AWS Config**                           | Kiểm soát tuân thủ cấu hình bucket             |
| **AWS Cost Explorer**                    | Giám sát chi phí lưu trữ                       |
| **IAM (Identity and Access Management)** | Quản lý quyền truy cập và bảo mật              |

---

### 🔄 Component Interactions & Data Flow

1. Người dùng tải dữ liệu lên **S3 bucket** đã cấu hình.
2. Dữ liệu ban đầu nằm ở lớp **Frequent Access** (trong Intelligent-Tiering).
3. Sau 30 ngày không truy cập, dữ liệu tự động chuyển sang lớp **Infrequent Access**.
4. Sau 90 ngày không truy cập, dữ liệu tiếp tục được đẩy xuống lớp **Glacier**.
5. Trong quá trình này, **Object Lock** đảm bảo rằng dữ liệu không bị xóa hay ghi đè ngoài ý muốn.
6. Người dùng có thể giám sát trạng thái chuyển tầng qua **S3 Analytics**, **CloudWatch**, và dự báo chi phí bằng **Cost Explorer**.

---

### 🔐 Security Architecture & Compliance

| Tính năng bảo mật                   | Mô tả                                                        |
| ----------------------------------- | ------------------------------------------------------------ |
| **S3 Default Encryption (AES-256)** | Mọi dữ liệu đều được mã hóa tự động                          |
| **S3 Bucket Policy**                | Chỉ định quyền truy cập dựa trên vai trò                     |
| **IAM Policy**                      | Người dùng chỉ được quyền upload và đọc file, không được xóa |
| **Object Lock – Governance Mode**   | Không thể xóa file trong thời gian khóa, kể cả admin         |
| **Versioning**                      | Bật versioning để phục hồi phiên bản cũ nếu có lỗi ghi đè    |

Compliance trong đề tài tập trung vào:

* Giữ dữ liệu tối thiểu 7 ngày.
* Ngăn xóa ngoài ý muốn.
* Có nhật ký truy cập và cảnh báo.

---

### ⚙️ Scalability & Performance

Vì sử dụng dịch vụ AWS S3, hệ thống:

* **Tự động mở rộng** khi có thêm dữ liệu.
* Không yêu cầu cấu hình máy chủ hoặc dung lượng cứng.
* **Tốc độ truy cập cao** cho dữ liệu thường xuyên.
* **Khả năng phục hồi dữ liệu** từ Glacier trong vài giờ.

Đối với bài toán học thuật, khả năng mở rộng về logic và công nghệ được ưu tiên hơn khía cạnh hạ tầng vật lý.

---

### 🔌 Integration Points with Existing Systems

Mặc dù đề tài thực hiện trên môi trường giả lập, hệ thống có thể mở rộng và tích hợp với:

* **Backup Script nội bộ** (Python, Bash) sử dụng AWS CLI hoặc SDK.
* **Phần mềm backup bên thứ ba** như Veeam, Commvault nếu triển khai thực tế.
* **Công cụ DevOps / CI-CD** để tự động hóa lưu trữ backup.

---

🎯 **Tổng kết phần kiến trúc:**

Hệ thống được thiết kế đơn giản, hiện đại, tận dụng các tính năng sẵn có của AWS để đáp ứng đầy đủ các yêu cầu: tự động, bảo mật, chi phí thấp và khả năng mở rộng linh hoạt.


## 🔧 4. Technical Implementation (20%)

### 📑 Implementation Phases

Quá trình triển khai được chia thành 6 giai đoạn rõ ràng, mỗi giai đoạn đều có mục tiêu cụ thể và kết quả đầu ra (deliverable):

| Giai đoạn                      | Thời gian | Nội dung                                                   |
| ------------------------------ | --------- | ---------------------------------------------------------- |
| 1. Chuẩn bị môi trường         | 1–2 ngày  | Tạo tài khoản AWS, kích hoạt Free Tier, kiểm tra billing   |
| 2. Tạo và cấu hình S3 Bucket   | 2–3 ngày  | Tạo bucket, bật Object Lock, kích hoạt Intelligent-Tiering |
| 3. Upload dữ liệu mẫu          | 2 ngày    | Đưa lên các file hình ảnh nhỏ (JPEG/PNG), dung lượng <5MB  |
| 4. Giám sát chuyển tầng        | 7 ngày    | Theo dõi truy cập file, chờ AWS chuyển tầng tự động        |
| 5. Kiểm thử khôi phục & bảo vệ | 3 ngày    | Thử khôi phục file từ Glacier, thử xóa file bị khóa        |
| 6. Tổng hợp báo cáo            | 2 ngày    | Ghi nhận kết quả, chụp ảnh minh họa, phân tích chi phí     |

---

### 💻 Technical Requirements

| Yêu cầu                        | Mô tả                                                  |
| ------------------------------ | ------------------------------------------------------ |
| **Tài khoản AWS**              | Có thể dùng Free Tier (miễn phí 12 tháng với giới hạn) |
| **Giao diện thao tác**         | AWS Console (giao diện web), hoặc AWS CLI              |
| **Dữ liệu mẫu**                | Các file hình ảnh từ 500KB đến 5MB, khoảng 50–100 file |
| **Máy tính cá nhân**           | Có kết nối internet, trình duyệt web                   |
| **Công cụ bổ sung (tuỳ chọn)** | AWS CLI, Python boto3 SDK, VSCode                      |

---

### 🛠️ Development Approach

Triển khai thủ công thông qua **AWS Management Console** là lựa chọn phù hợp với sinh viên và người mới học. Đối với người muốn nâng cao kỹ năng, có thể viết thêm **script tự động hóa** bằng AWS CLI hoặc Python SDK.

Ngoài ra, nếu có kỹ năng hạ tầng, có thể:

* Viết file `CloudFormation` để tự động tạo Bucket + Policy.
* Dùng **Terraform** cho quản lý cấu hình hạ tầng (không bắt buộc).

---

### 🧪 Testing Strategy

#### ✅ Unit Testing (thủ công):

* Kiểm tra từng bước cấu hình: bật Object Lock, kiểm tra Tier hiện tại.

#### 🔄 Integration Testing:

* Tải lên file → Theo dõi CloudWatch → Kiểm tra chuyển tầng.
* Thử xóa file khi đang bật Object Lock (phải thất bại).

#### 🚀 Performance Simulation:

* Truy cập một số file nhiều lần để giữ ở lớp “frequent”.
* Không truy cập các file khác để ép chúng rơi vào lớp “infrequent” hoặc Glacier.

#### 🛡️ Compliance Check:

* Đảm bảo không xóa được file trong thời gian khóa.
* Kiểm tra nhật ký hoạt động từ AWS Config.

---

### 🚀 Deployment Plan & Rollback

| Hoạt động               | Kế hoạch                                             |
| ----------------------- | ---------------------------------------------------- |
| **Triển khai**          | Trực tiếp bằng tay trên AWS Console, không cần CI/CD |
| **Rollback (phục hồi)** | Dùng S3 Versioning nếu file bị ghi đè                |
| **Restore từ Glacier**  | Sử dụng tính năng "Restore" trên file đã chuyển tầng |


---

### 🧰 Configuration Management

* **Tên bucket** đặt theo chuẩn `backup-username-ddmmyy`.
* **Cấu hình lifecycle policy** lưu trữ file trong Glacier sau 90 ngày.
* **Lưu lại log thao tác**: chụp ảnh màn hình từng bước cấu hình.
* **Theo dõi chuyển tầng** qua tab *Storage class transitions* trong AWS Console.

## 📅 5. Timeline & Milestones (10%)

### 📆 Project Timeline

Toàn bộ dự án được thiết kế để hoàn thành trong **20 ngày**, chia thành 5 giai đoạn chính, mỗi giai đoạn kéo dài từ 2 đến 5 ngày, phù hợp với tiến độ tự học hoặc thực tập bán thời gian.

| Giai đoạn   | Thời gian  | Mô tả                                                 |
| ----------- | ---------- | ----------------------------------------------------- |
| Giai đoạn 1 | Ngày 1–2   | Khởi tạo tài khoản AWS, làm quen giao diện            |
| Giai đoạn 2 | Ngày 3–5   | Cấu hình S3 Bucket, bật Object Lock, upload file mẫu  |
| Giai đoạn 3 | Ngày 6–12  | Theo dõi truy cập và quan sát chuyển tầng dữ liệu     |
| Giai đoạn 4 | Ngày 13–17 | Kiểm thử phục hồi từ Glacier, kiểm tra bảo vệ dữ liệu |
| Giai đoạn 5 | Ngày 18–20 | Tổng hợp kết quả, phân tích chi phí, viết báo cáo     |

---

### 🎯 Key Milestones

| Mốc | Mô tả                                         | Tiêu chí hoàn thành                                    |
| --- | --------------------------------------------- | ------------------------------------------------------ |
| M1  | Tạo thành công bucket với Object Lock         | Có ảnh chụp xác nhận                                   |
| M2  | Upload và xác nhận file được lưu trữ đúng lớp | Kiểm tra Storage Class = Intelligent-Tiering           |
| M3  | Chuyển tầng thành công (sau 7 ngày)           | Một số file chuyển sang Infrequent hoặc Glacier        |
| M4  | Thực hiện phục hồi từ Glacier                 | File hiển thị trạng thái "Restoring" hoặc "Restored"   |
| M5  | Viết xong báo cáo, tổng hợp kết quả           | Báo cáo đầy đủ nội dung, có minh họa ảnh chụp màn hình |

---

### 🧩 Dependencies

| Thành phần phụ thuộc      | Mô tả                                                                                                                                           |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **Thời gian chuyển tầng** | AWS yêu cầu \~30 ngày không truy cập để chuyển sang lớp lưu trữ lạnh. Có thể mô phỏng bằng cách chờ ít nhất 7–10 ngày để thấy thay đổi ban đầu. |
| **Mạng và tài khoản**     | Cần internet ổn định, AWS Free Tier còn hạn mức.                                                                                                |
| **File mẫu**              | Cần sẵn các tệp nhỏ (<5MB), nhiều định dạng (.jpg, .png, .zip, .txt) để tạo dữ liệu đầu vào.                                                    |

---

### 👥 Resource Allocation

* **Nhân lực:** 1 sinh viên thực hiện toàn bộ quá trình
* **Hỗ trợ:** Có thể tham khảo tài liệu hướng dẫn AWS chính thức hoặc sử dụng ChatGPT nếu gặp lỗi
* **Thời gian làm việc:** Có thể phân chia thời gian linh hoạt (toàn thời gian hoặc 2–3 giờ mỗi ngày)

---

### 🛑 Buffer Time (1–2 ngày)

Dự phòng cho các rủi ro:

* Kết nối internet chậm hoặc lỗi từ phía AWS
* Quên thao tác bật tính năng (ví dụ Object Lock)
* Delay từ AWS trong việc chuyển tầng (Glacier delay \~1–3 ngày)


## 💰 6. Budget Estimation (10%)

### 💡 Nguyên tắc tính toán

Đề tài này sử dụng **AWS Free Tier** cho người mới, do đó **chi phí thực tế gần như bằng 0** nếu đảm bảo:

* Tổng dung lượng sử dụng **< 5GB**
* Thời gian triển khai **< 30 ngày**
* Số lượng thao tác (PUT/GET) trong giới hạn miễn phí (\~20.000 yêu cầu/tháng)

Chi phí chỉ được giả lập, mục tiêu là để người học hiểu rõ cơ chế tính tiền, cách đọc billing, và phương pháp dự phòng nếu triển khai thật sự.

---

### 📦 Infrastructure Costs (AWS Usage)

| Dịch vụ AWS            | Miễn phí Free Tier            | Ước lượng sử dụng  | Ước tính chi phí    |
| ---------------------- | ----------------------------- | ------------------ | ------------------- |
| S3 Standard            | 5GB miễn phí/tháng            | \~1–2 GB           | 0 VND               |
| S3 Intelligent-Tiering | Không phụ phí khi bật         | \~1–2 GB           | 0 VND               |
| Glacier                | Phí thấp (\~\$0.004/GB/tháng) | \~500MB            | \~0.002 USD         |
| PUT/GET requests       | 20K miễn phí                  | \~2000             | 0 VND               |
| CloudWatch             | Miễn phí 10 custom metrics    | Sử dụng basic logs | 0 VND               |
| **Tổng cộng**          |                               |                    | **\~0 – 1.000 VNĐ** |

> 🎯 **Kết luận:** Với khối lượng thử nghiệm nhỏ, đề tài không phát sinh chi phí thực tế. Tuy nhiên, người học sẽ hiểu sâu cách đọc bảng giá AWS và cách tính billing.

---

### 🛠️ Development Costs

| Hạng mục                  | Nội dung                              | Chi phí |
| ------------------------- | ------------------------------------- | ------- |
| Tài khoản AWS             | Đăng ký miễn phí                      | 0 VND   |
| Máy tính cá nhân          | Sẵn có                                | 0 VND   |
| Hướng dẫn & tài liệu      | Dùng tài liệu AWS & hỗ trợ từ ChatGPT | 0 VND   |
| Thời gian người thực hiện | 20 ngày (\~2 giờ/ngày)                | 0 VND   |


---

### 🔄 Operational Costs (nếu triển khai thực tế)

| Chi phí vận hành          | Mô tả                          | Ước tính              |
| ------------------------- | ------------------------------ | --------------------- |
| Lưu trữ lâu dài > 30 ngày | Nếu vượt Free Tier             | \~0.02–0.50 USD/tháng |
| Truy xuất từ Glacier      | \~0.03 USD/GB                  | Không bắt buộc        |
| CloudWatch nâng cao       | 0.30–0.50 USD/tháng            | Không dùng            |
| **Tổng chi phí vận hành** | Chỉ phát sinh khi chạy thật sự | **< 1 USD/tháng**     |

---

### 📊 ROI Analysis (Lợi ích đầu tư)

Mặc dù không phát sinh chi phí thực, đề tài mang lại những giá trị thực tiễn:

| Hạng mục                     | Giá trị                                     |
| ---------------------------- | ------------------------------------------- |
| Kiến thức thực hành AWS      | Tự tay cấu hình S3, Glacier, Object Lock    |
| Kỹ năng tối ưu lưu trữ       | Biết cách tiết kiệm chi phí từ tầng lưu trữ |
| Tư duy bảo mật dữ liệu       | Thực hành chống xóa nhầm, kiểm tra WORM     |
| Áp dụng cho doanh nghiệp nhỏ | Có thể triển khai ngay nếu cần              |


---

### 💡 Cost Optimization Strategies

1. **Dùng Intelligent-Tiering thay vì S3 Standard:**
   → Giúp tiết kiệm \~40–70% cho dữ liệu ít truy cập.

2. **Bật Object Lock thay vì tạo hệ thống backup riêng:**
   → Không cần dịch vụ backup ngoài, tiết kiệm vận hành.

3. **Xem billing thường xuyên qua Cost Explorer:**
   → Theo dõi và cắt giảm khi cần.

4. **Không giữ dữ liệu quá 30 ngày khi test:**
   → Tránh bị tính phí ngoài Free Tier.


## ⚠️ 7. Risk Assessment (5%)

### 📋 Risk Identification

| Rủi ro                                                     | Mô tả ngắn                                                            |
| ---------------------------------------------------------- | --------------------------------------------------------------------- |
| **R1. Quên bật Object Lock**                               | Dữ liệu có thể bị xóa hoặc ghi đè khi chưa bật chế độ khóa dữ liệu    |
| **R2. Không đủ thời gian để kiểm tra chuyển tầng**         | AWS cần 30 ngày để tự động chuyển lớp lưu trữ sang Infrequent/Glacier |
| **R3. Thiếu dữ liệu thử nghiệm đa dạng**                   | Việc kiểm tra tiering không hiệu quả nếu chỉ có một vài file nhỏ      |
| **R4. Không kiểm tra được khôi phục từ Glacier**           | Có thể quên bật Glacier hoặc không chờ đủ thời gian restore           |
| **R5. Sai quyền truy cập IAM/Bucket Policy**               | Người dùng có thể tự xóa file nếu cấu hình sai policy                 |
| **R6. Chi phí phát sinh nếu quên xóa dữ liệu sau 30 ngày** | Có thể bị tính phí ngoài Free Tier                                    |

---

### 🧮 Impact Assessment & Probability

| Rủi ro | Khả năng xảy ra | Mức độ ảnh hưởng |
| ------ | --------------- | ---------------- |
| R1     | Trung bình      | Cao              |
| R2     | Cao             | Trung bình       |
| R3     | Trung bình      | Trung bình       |
| R4     | Thấp            | Trung bình       |
| R5     | Trung bình      | Cao              |
| R6     | Thấp            | Thấp             |

> 🎯 **R1 và R5** là hai rủi ro quan trọng nhất cần kiểm soát ngay từ đầu.

---

### 📊 Risk Matrix

|                         | **Ảnh hưởng Thấp** | **Ảnh hưởng Trung Bình** | **Ảnh hưởng Cao** |
| ----------------------- | ------------------ | ------------------------ | ----------------- |
| **Khả năng Thấp**       | R6                 | R4                       |                   |
| **Khả năng Trung bình** | R3                 |                          | R1, R5            |
| **Khả năng Cao**        |                    | R2                       |                   |

---

### 🛡️ Mitigation Strategies

| Rủi ro | Giải pháp phòng ngừa                                                                      |
| ------ | ----------------------------------------------------------------------------------------- |
| **R1** | Bật Object Lock ngay sau khi tạo bucket; kiểm tra trên AWS Console                        |
| **R2** | Mô phỏng bằng cách upload file, không truy cập và chờ ≥7 ngày để theo dõi chuyển lớp      |
| **R3** | Chuẩn bị ít nhất 50 file với các loại định dạng khác nhau (.jpg, .txt, .zip…)             |
| **R4** | Chủ động bật Glacier transition; thử restore bằng tay và chụp màn hình                    |
| **R5** | Tạo IAM user riêng, chỉ cấp quyền `s3:PutObject` và `s3:GetObject`, cấm `s3:DeleteObject` |
| **R6** | Đặt lịch nhắc kiểm tra billing; xóa dữ liệu trước khi hết 30 ngày thử nghiệm              |

---

### 📦 Contingency Plans

| Rủi ro | Kế hoạch dự phòng                                                                                 |
| ------ | ------------------------------------------------------------------------------------------------- |
| **R1** | Nếu quên bật Object Lock, ghi chú trong báo cáo là lỗi phổ biến, phân tích hậu quả                |
| **R2** | Nếu không đủ thời gian quan sát chuyển tầng, bổ sung mô phỏng hoặc chụp ảnh trạng thái hiện tại   |
| **R4** | Nếu Glacier không có file, mô tả quy trình restore bằng lý thuyết + hình minh họa từ tài liệu AWS |
| **R6** | Nếu có phát sinh chi phí, ghi lại số tiền và lý do cụ thể để rút kinh nghiệm cho lần sau          |

---

### 📣 Monitoring & Escalation Procedures

| Hoạt động                | Mô tả                                                                               |
| ------------------------ | ----------------------------------------------------------------------------------- |
| **Giám sát thủ công**    | Kiểm tra Storage Class và Object Lock qua giao diện AWS                             |
| **Thông báo (tùy chọn)** | Có thể cấu hình AWS Billing Alerts hoặc CloudWatch để gửi cảnh báo (không bắt buộc) |
| **Log thao tác**         | Chụp ảnh từng bước làm việc, lưu lại log thao tác để dễ kiểm tra và trình bày       |


## 🎯 8. Expected Outcomes (5%)

### 📌 Success Metrics

| Mục tiêu                          | Chỉ tiêu đo lường cụ thể                                                |
| --------------------------------- | ----------------------------------------------------------------------- |
| ✅ Triển khai hoàn chỉnh           | Đã tạo S3 Bucket, bật Object Lock, upload dữ liệu mẫu                   |
| ✅ Theo dõi chuyển tầng thành công | Một số file có Storage Class chuyển sang Infrequent Access hoặc Glacier |
| ✅ Kiểm tra khôi phục dữ liệu      | Tối thiểu 1 file được khôi phục từ Glacier thành công                   |
| ✅ Bảo vệ dữ liệu hiệu quả         | Không thể xóa file đã bật Object Lock trong thời gian giữ               |
| ✅ Viết báo cáo đầy đủ             | Bao gồm hình minh họa, phân tích kỹ thuật và chi phí                    |

---

### 🟢 Short-term Benefits (0–6 tháng)

| Lợi ích                                                 | Mô tả                                                        |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| 🧠 Hiểu rõ cơ chế phân tầng dữ liệu trong S3            | Biết cách kích hoạt, cấu hình và kiểm tra chuyển tầng        |
| 🔒 Nắm vững cách bảo vệ dữ liệu bằng Object Lock        | Tự tay thao tác và thấy hiệu quả chống xóa nhầm              |
| 💸 Nhận thức chi phí lưu trữ và cách tối ưu             | Biết so sánh giữa Standard vs Intelligent-Tiering vs Glacier |
| 📊 Biết cách đọc log truy cập, dùng CloudWatch, Billing | Có kiến thức cơ bản về monitoring trong AWS                  |
| 📁 Chuẩn bị nền tảng kiến thức cho các dự án lớn hơn    | Cấu hình chuẩn để phát triển các hệ thống backup thực tế     |

---

### 🟡 Medium-term Benefits (6–18 tháng)

| Lợi ích                                                             | Mô tả                                                    |
| ------------------------------------------------------------------- | -------------------------------------------------------- |
| 📦 Áp dụng cho môi trường doanh nghiệp nhỏ hoặc startup             | Có thể triển khai mô hình sao lưu đơn giản trong thực tế |
| 🔄 Hỗ trợ các nhóm DevOps, Data Engineering quản lý dữ liệu         | Hiểu được quy trình lưu trữ – bảo vệ – khôi phục dữ liệu |
| 📚 Nâng cao kỹ năng thiết kế kiến trúc lưu trữ                      | Có tư duy chọn lớp lưu trữ theo đặc tính dữ liệu         |
| 🧩 Linh hoạt tích hợp thêm công cụ như AWS CLI, SDK, hoặc Terraform | Tăng tính tự động hóa và mở rộng triển khai thực tế      |

---

### 🔵 Long-term Value (18+ tháng)

| Lợi ích dài hạn                                                  | Mô tả                                                       |
| ---------------------------------------------------------------- | ----------------------------------------------------------- |
| 🧭 Hình thành tư duy Cloud-native Storage Management             | Dễ dàng triển khai các giải pháp lớn hơn trong doanh nghiệp |
| 🏛️ Đáp ứng các yêu cầu tuân thủ bảo mật, kiểm toán              | Biết áp dụng retention policy, WORM, phân quyền IAM         |
| 💼 Tăng khả năng làm việc với các vị trí Cloud, Data Ops, DevOps | Gây ấn tượng khi phỏng vấn, hỗ trợ nghề nghiệp lâu dài      |
| 💡 Góp phần xây dựng mô hình lưu trữ chiến lược, tiết kiệm       | Đóng vai trò tư vấn giải pháp tối ưu lưu trữ trong tổ chức  |

---

### 🧩 Strategic Capabilities Gained

* 🎯 Khả năng phân tích chi phí – hiệu quả lưu trữ
* 📚 Kỹ năng đọc tài liệu kỹ thuật AWS, áp dụng vào thực tế
* 🔐 Nhận thức đầy đủ về bảo vệ dữ liệu, quyền truy cập
* 🧪 Hiểu quy trình kiểm thử hệ thống lưu trữ trong cloud

# 📎 Appendices

## 📘 A. Technical Specifications

| Thành phần           | Cấu hình cụ thể                                                 |
| -------------------- | --------------------------------------------------------------- |
| **S3 Bucket**        | `backup-project-demo`                                           |
| **Storage Class**    | Intelligent-Tiering (bật mặc định khi upload)                   |
| **Object Lock**      | Bật chế độ **Governance**, thời gian giữ: 7–30 ngày             |
| **Encryption**       | SSE-S3 (mặc định), sử dụng AES-256                              |
| **Lifecycle Policy** | (Tùy chọn mở rộng): Di chuyển sang Glacier sau 30 ngày          |
| **IAM Roles**        | Cấp quyền `s3:GetObject`, `s3:PutObject`; cấm `s3:DeleteObject` |
| **Monitoring**       | CloudWatch để theo dõi lượt truy cập và hoạt động GET/PUT       |

---

## 📊 B. Cost Calculations (Mô phỏng)

### Ví dụ tính chi phí cho dữ liệu lưu 1 tháng:

| Loại lưu trữ               | Dung lượng | Giá tham khảo (USD/GB/tháng) | Tổng (ước tính)              |
| -------------------------- | ---------- | ---------------------------- | ---------------------------- |
| S3 Intelligent-Tiering     | 2 GB       | \$0.023                      | \$0.046                      |
| Glacier Flexible Retrieval | 1 GB       | \$0.004                      | \$0.004                      |
| CloudWatch Basic           | 10 logs    | Miễn phí                     | \$0.00                       |
| **Tổng cộng**              | **–**      | –                            | **\~0.05 USD (\~1.200 VNĐ)** |


---

## 🗺️ C. Architecture Diagrams

### ☁️ Sơ đồ kiến trúc hệ thống

```plaintext
┌─────────────────────────────────────────────┐
│               Người dùng / App              │
└────────────┬───────────────────────┬────────┘
             │                       │
             ▼                       ▼
    Upload dữ liệu         Truy vấn / restore
             │                       │
             ▼                       ▼
      ┌──────────────┐       ┌──────────────┐
      │  S3 Bucket   │──────▶│ Intelligent- │
      │ (Object Lock │       │ Tiering Logic│
      │ + Encryption)│◀──────│   (auto move)│
      └──────┬───────┘       └──────────────┘
             │
             ▼
     ┌────────────────┐
     │ Glacier Storage│
     └────────────────┘
```

---

## 🔗 D. References

| Chủ đề                      | Link                                                                                                                                                                                         |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S3 Intelligent-Tiering      | [https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intelligent-tiering.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intelligent-tiering.html) |
| Object Lock & Compliance    | [https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html)                           |
| Glacier Restore Process     | [https://docs.aws.amazon.com/AmazonS3/latest/userguide/restoring-objects.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/restoring-objects.html)                                 |
| AWS Free Tier               | [https://aws.amazon.com/free](https://aws.amazon.com/free)                                                                                                                                   |
| IAM Policy Reference        | [https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)                                              |
| AWS Cost Calculator         | [https://calculator.aws.amazon.com](https://calculator.aws.amazon.com)                                                                                                                       |
| AWS Monitoring & CloudWatch | [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)                 |

---
