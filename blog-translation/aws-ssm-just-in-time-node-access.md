# 🎯 GIỚI THIỆU QUYỀN TRUY CẬP NÚT JUST-IN-TIME BẰNG AWS SYSTEMS MANAGER

**Tác giả:** Chetan Makvana, Anthony Verleysen, Mark Brealey  
**Ngày:** 29 tháng 4 năm 2025  
**Chủ đề:** Thông báo, AWS Systems Manager, Bảo mật, Không cần máy chủ  

## 🛡 Tổng quan

Quyền truy cập nút Just-in-Time (JIT) là tính năng mới trong **AWS Systems Manager** giúp:

- Truy cập tạm thời, có kiểm soát vào các node EC2, tại chỗ và đa đám mây.
- Hỗ trợ chính sách phê duyệt tự động hoặc thủ công.
- Loại bỏ nhu cầu về thông tin xác thực dài hạn.
- Tăng cường bảo mật và hiệu quả vận hành.

## 🔐 Lợi ích chính

- **Giảm nguy cơ bảo mật:** Không còn thông tin xác thực dài hạn.
- **Kiểm soát truy cập linh hoạt:** Chỉ định rõ người dùng nào được truy cập node nào, trong điều kiện nào.
- **Hỗ trợ đa kênh:** Slack, Teams, Email.
- **Ghi nhật ký:** Command log và RDP session log.

## ⚙️ Các vai trò chính

- **Quản trị viên:** Tạo và duy trì chính sách phê duyệt.
- **Người vận hành (Operator):** Yêu cầu quyền truy cập.
- **Người phê duyệt (Approver):** Xem và chấp thuận từ bảng điều khiển hoặc CLI.

## 🚀 Thiết lập truy cập nút JIT

### Bước 1 – Bật Just-in-Time Node Access

- Thiết lập **Systems Manager unified dashboard**.
- Kích hoạt JIT cho toàn bộ tổ chức hoặc một OU cụ thể.

### Bước 2 – Tạo chính sách phê duyệt

Có 3 loại chính sách:

- **Tự động (Auto):** Không cần phê duyệt.
- **Thủ công (Manual):** Cần xác nhận bởi người phê duyệt.
- **Từ chối (Deny):** Từ chối mọi yêu cầu.

Chính sách được xác định qua:

- Tên, mô tả, thời hạn truy cập.
- Thẻ (Tag) để nhắm mục tiêu node.
- Danh sách người phê duyệt.

Ví dụ chính sách Cedar:

```cedar
permit (
    principal in AWS::IdentityStore::Group::"911b8590-7041-70fa-d20b-12345EXAMPLE",
    action == AWS::SSM::Action::"getTokenForInstanceAccess", 
    resource)
  when {
    resource.hasTag("Environment") && 
    resource.getTag("Environment") == "Development"
  };
```

## 📥 Quá trình yêu cầu truy cập

- Người vận hành gửi yêu cầu truy cập (kèm lý do).
- Hệ thống thông báo tới người phê duyệt.
- Người vận hành có thể theo dõi tiến trình trong tab "Access Requests".

## ✅ Phê duyệt & kết nối

- Người phê duyệt có thể thao tác qua bảng điều khiển hoặc CLI.
- Sau khi được chấp thuận, người vận hành kết nối node qua CLI hoặc Session Manager.

## 💰 Giá cả & Thời gian dùng thử

- Có bản dùng thử miễn phí 1 kỳ + phần còn lại của kỳ hiện tại.
- Sau đó tính phí theo mức sử dụng.
- Xem chi tiết tại: [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)

## 📚 Kết luận

Just-in-Time Node Access giúp cân bằng giữa bảo mật và hiệu quả hoạt động. Tích hợp tốt với hệ thống hiện có, đồng thời hỗ trợ quản lý truy cập đa tài khoản, đa vùng và ghi lại đầy đủ nhật ký hoạt động.

---

**Tác giả:**  
- *Chetan Makvana* – Enterprise Solutions Architect tại AWS  
- *Mark Brealey* – Senior Migration SA tại AWS  
- *Anthony Verleysen* – Sr. Product Manager tại AWS Systems Manager

