# AWS Lambda: Chuẩn Hóa Việc Thanh Toán Cho Giai Đoạn INIT

**Tác giả**: Shubham Gupta và Jeff Gebhart  
**Ngày đăng**: 29 Tháng 4 năm 2025  
**Thể loại**: Thông báo, AWS Lambda, Trung cấp (200), Serverless  

---

## 🎯 Thay đổi chính

Có hiệu lực từ **ngày 1 tháng 8 năm 2025**, AWS sẽ **tính phí cho giai đoạn INIT** trên tất cả các hàm Lambda, bao gồm cả hàm sử dụng thời gian chạy được quản lý với gói ZIP. Trước đây, INIT không được tính phí với loại hàm này.

---

## 🔄 Vòng đời hàm Lambda

Gồm 3 giai đoạn:

1. **INIT**: tạo môi trường thực thi, tải mã, khởi tạo extension, runtime, hàm INIT.
2. **INVOKE**: xử lý yêu cầu.
3. **SHUTDOWN**: hủy môi trường.

Giai đoạn INIT bao gồm:

- Khởi tạo extension
- Khởi động runtime
- Thực thi mã tĩnh
- Hook SnapStart (nếu có)

---

## 💰 Chi tiết thanh toán INIT

**Trước đây**:

```
Duration: 250.06 ms | Billed Duration: 251 ms | Init Duration: 100.77 ms
```

**Sau ngày 01/08/2025**:

```
Duration: 250.06 ms | Billed Duration: 351 ms | Init Duration: 100.77 ms
```

### Ảnh hưởng:

- INIT sẽ được tính theo giá chuẩn của Lambda theo vùng.
- Lambda@Edge sẽ tính phí INIT theo giá riêng của nó.

---

## 📊 Theo dõi INIT Duration

Dùng CloudWatch Logs và Metric `init_duration`. Truy vấn mẫu CloudWatch Logs Insights:

```sql
filter @type = "REPORT" and @billedDuration < (@duration + @initDuration) 
| stats 
  sum((@memorySize/1000000/1024) * (@billedDuration/1000)) as BilledGBs, 
  sum((@memorySize/1000000/1024) * ((ceil(@duration + @initDuration) - @billedDuration)/1000)) as UnbilledInitGBs, 
  (UnbilledInitGBs / (UnbilledInitGBs + BilledGBs)) as Ratio
```

Kết quả:

- **BilledGBs**: GB-s bị tính phí
- **UnbilledInitGBs**: GB-s INIT chưa tính phí
- **Ratio**: tỷ lệ INIT chưa tính phí

---

## ⚙️ Chiến lược tối ưu hóa INIT

### ✅ Khi nào INIT xảy ra?

- Tạo môi trường mới (cold start)
- Mở rộng hàm Lambda

### ✅ Tối ưu trong INIT

- Tải thư viện dùng chung
- Kết nối tới S3, DynamoDB, database
- Truy vấn secrets/parameter trước

### ✅ Giảm thời gian INIT

- Tối ưu kích thước gói
- Loại bỏ thư viện không cần thiết
- Dùng công cụ như `esbuild`

---

## 🚀 Tính năng hỗ trợ tối ưu INIT

### 1. Lambda SnapStart

- Chụp ảnh sau INIT đầu tiên → dùng lại
- Giảm chi phí INIT cho các lần cold start sau
- Hỗ trợ Java, .NET, Python

### 2. Provisioned Concurrency

- Khởi tạo môi trường trước khi gọi
- Tránh INIT hoàn toàn khi gọi hàm
- Phù hợp cho workload liên tục (> 60%)

---

## 🧠 Kết luận

Từ 01/08/2025, INIT sẽ được tính phí trên tất cả các loại hàm Lambda. Hãy:

- Theo dõi INIT Duration
- Tối ưu mã khởi tạo
- Xem xét SnapStart & Provisioned Concurrency

**Tài nguyên khuyến nghị**:

- [AWS Lambda Pricing](https://aws.amazon.com/lambda/pricing/)
- [Lambda Execution Environment Lifecycle](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-lifecycle.html)
- [Lambda SnapStart](https://docs.aws.amazon.com/lambda/latest/dg/snapstart.html)

---

> 📌 **Ghi chú**: Hãy làm việc với AWS Support để lựa chọn chiến lược tối ưu chi phí phù hợp với workload cụ thể.
