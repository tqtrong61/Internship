# 📘 Worklog - Ngày 14/05/2025

## 📅 Thông tin cơ bản

- **Ngày**: 14/05/2025  
- **Thứ**: Thứ Tư  
- **Tuần thực tập**: Tuần thứ 1/12  
- **Thời gian làm việc**: 9:00 - 17:00  
- **Mood**: 💰 + Hứng thú tìm hiểu cách quản lý chi phí AWS hiệu quả

---

## 🎯 Mục tiêu ngày hôm nay

- Tìm hiểu cách thiết lập **AWS Budgets** để theo dõi và kiểm soát chi phí dịch vụ
- Tạo ngân sách giới hạn cho tài khoản AWS cá nhân
- Thiết lập cảnh báo (alerts) khi vượt quá ngân sách
- Hiểu cách sử dụng Cost Explorer kết hợp với Budgets để phân tích chi phí

---

## 💼 Công việc đã thực hiện

### 1. Làm quen với giao diện AWS Budgets ⏱️ 2 giờ

- **Mô tả**: Truy cập dịch vụ AWS Budgets từ Billing console, tìm hiểu các loại ngân sách hỗ trợ
- **Kết quả**: Nắm được 3 loại ngân sách chính: Cost Budget, Usage Budget, Reservation Budget
- **Tools/Tech**: AWS Management Console, AWS Budgets
- **Links**: [AWS Budgets Overview](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)

---

### 2. Tạo Cost Budget cá nhân theo tháng ⏱️ 2 giờ

- **Mô tả**: Thiết lập ngân sách 10 USD/tháng để theo dõi chi phí học tập AWS
- **Kết quả**: Khi chi phí đạt 80%, sẽ có email cảnh báo gửi về
- **Tools/Tech**: AWS Budgets, Amazon SNS (để nhận alert)
- **Links**: [Hướng dẫn tạo AWS Budget](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-create.html)

---

### 3. Phân tích chi phí thực tế qua Cost Explorer ⏱️ 2 giờ

- **Mô tả**: Dùng Cost Explorer để xem dịch vụ nào đang tiêu tốn ngân sách nhiều nhất
- **Kết quả**: Nhận ra EC2 chiếm hơn 70% chi phí do chưa tắt instance không dùng
- **Tools/Tech**: AWS Cost Explorer, Tags
- **Links**: [Cost Explorer Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html)

---

## 📚 Kiến thức học được

### 🔧 Technical Skills

- Biết cách thiết lập **Cost Budget** trong AWS Budgets
- Dùng **Amazon SNS** để nhận email cảnh báo khi vượt ngân sách
- Phân tích chi phí chi tiết bằng **Cost Explorer**

### 💡 Concepts & Theory

- Chi phí AWS có thể được giám sát theo **usage**, **cost**, hoặc **reservation**
- AWS Budget hỗ trợ thiết lập theo **monthly**, **quarterly**, hoặc **custom period**
- Budget Alerts giúp **phòng ngừa chi phí vượt kiểm soát**, nhất là với người mới

### 🤝 Soft Skills

- Tư duy quản lý tài chính khi làm việc trên môi trường cloud
- Cẩn thận khi bật/tắt dịch vụ để tránh lãng phí tài nguyên
- Kỹ năng đọc biểu đồ và tổng hợp thông tin từ Cost Explorer

---

## 🚧 Khó khăn và giải pháp

### Vấn đề 1: Không nhận được email cảnh báo

- **Mô tả**: Đã thiết lập alert nhưng không nhận được thông báo
- **Impact**: Không biết khi nào vượt ngân sách
- **Root Cause**: SNS Topic chưa được xác nhận email
- **Solution**: Vào hộp thư, nhấn “Confirm Subscription”
- **Result**: Sau đó nhận cảnh báo đầy đủ khi vượt 80%
- **Lesson**: Cần xác nhận SNS subscription thì mới nhận được cảnh báo

---

### Vấn đề 2: Không biết dịch vụ nào tiêu tốn nhiều chi phí

- **Mô tả**: Tổng chi phí tăng nhưng không biết nguyên nhân
- **Impact**: Khó kiểm soát ngân sách
- **Root Cause**: Không tag tài nguyên và không dùng Cost Explorer
- **Solution**: Bật Cost Explorer, lọc theo dịch vụ → nhận ra EC2 đang chạy liên tục
- **Result**: Tắt các instance không dùng → giảm chi phí đáng kể
- **Lesson**: Phân tích chi phí thường xuyên giúp tối ưu hóa ngân sách

---

## 💭 Reflection & Insights

### ✅ What went well today?

- Thiết lập được ngân sách cảnh báo khi vượt 80% usage
- Biết cách dùng Cost Explorer để theo dõi các dịch vụ ngốn tiền
- Tắt được EC2 thừa, tiết kiệm gần 3 USD/tháng

### 🔄 What could be improved?

- Cần thử áp dụng **Usage Budget** để theo dõi dung lượng sử dụng (GB, giờ...)
- Nên kết hợp thêm **Tag-based Budgets** để theo dõi chi phí theo project
- Tìm hiểu thêm về **Savings Plan** để dự đoán chi phí dài hạn

### 💡 Key Insights

- Quản lý chi phí cloud không thể làm một lần → cần **theo dõi liên tục**
- AWS Budgets + SNS Alerts là **công cụ cực kỳ hữu ích cho người mới**
- Biết rõ chi phí giúp mình tự tin hơn khi dùng AWS, không sợ “quá tay” với ngân sách

Worklog created by: Tran Quang Trong
Next review: 15/05/2025

