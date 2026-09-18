# LAB 2: HỆ THỐNG QUẢN LÝ THƯ VIỆN

## CẤU TRÚC BÀI NỘP
- `/DATABASE`: Bảng thiết kế CSDL và Script SQL
- `/UML`: Các sơ đồ thiết kế UML (Use Case, Class Diagram)
- `/FORMS`: Tài liệu mô tả / File báo cáo thiết kế Form giao diện

## KẾT QUẢ KIỂM THỬ (TEST CASES)
| Mã TC | Chức năng | Điều kiện kiểm thử | Kết quả |
| :--- | :--- | :--- | :--- |
| TC01 | Cấp thẻ | Độc giả chưa từng có thẻ | Thành công |
| TC02 | Cấp thẻ | Độc giả đang có thẻ active | Báo lỗi |
| TC03 | Mượn sách | Mượn tối đa 3 cuốn hợp lệ | Thành công |
| TC04 | Trả sách | Trả trễ/hư hỏng | Tự động tạo PhieuPhat |
