# TÀI LIỆU CHI TIẾT THIẾT KẾ FORM VÀ QUY TRÌNH NGHIỆP VỤ (LAB 2)

---

## 1. MÔ TẢ CHI TIẾT USE CASE (USE CASE SPECIFICATION)

### 1.1. Use Case: Lập Phiếu Mượn Sách (`UC_MuonSach`)

- **Actor:** Thủ thư.
- **Mục tiêu:** Ghi nhận thông tin mượn sách của độc giả và cập nhật số lượng sách tồn kho.
- **Tiền điều kiện (Pre-conditions):** 
  - Thủ thư đã đăng nhập vào hệ thống.
  - Độc giả trình thẻ thư viện còn hạn sử dụng (`TrangThai = 1`, `HanSuDung >= NgayHienTai`).
- **Hậu điều kiện (Post-conditions):**
  - Tạo mới 01 bản ghi trong bảng `PhieuMuon`.
  - Tạo các bản ghi trong bảng `ChiTietPhieuMuon`.
  - Giảm `SoLuongHienCo` của các đầu sách tương ứng trong bảng `DauSach`.

#### Luồng sự kiện chính (Basic Flow):
1. Thủ thư mở **Form Mượn Sách** và nhập `MaDocGia`.
2. Hệ thống kiểm tra thẻ độc giả và hiển thị thông tin độc giả (Họ tên, Trạng thái thẻ, Số sách đang mượn).
3. Thủ thư chọn/nhập mã các đầu sách độc giả muốn mượn (Tối đa 3 cuốn).
4. Hệ thống kiểm tra số lượng tồn kho (`SoLuongHienCo > 0`) của từng đầu sách.
5. Thủ thư nhấn nút **"Lập phiếu mượn"**.
6. Hệ thống lưu `PhieuMuon`, `ChiTietPhieuMuon`, cập nhật tồn kho sách và hiển thị thông báo thành công.

#### Luồng ngoại lệ (Alternative / Exception Flows):
- **4a. Thẻ độc giả hết hạn hoặc bị khóa:** Hệ thống hiển thị thông báo lỗi *"Thẻ không hợp lệ hoặc đã hết hạn"* và chặn không cho mượn.
- **4b. Độc giả đang nợ sách quá hạn:** Hệ thống hiển thị thông báo *"Độc giả có sách quá hạn chưa trả"* và dừng quy trình.
- **4c. Sách đã hết trong kho (`SoLuongHienCo = 0`):** Hệ thống báo *"Đầu sách [X] đã hết trong kho"* và yêu cầu chọn sách khác.

---

## 2. SEQUENCE DIAGRAM: FORM NHẬN TRẢ SÁCH & PHẠT (`FormTraSach`)

```plantuml
@startuml
autonumber
actor "Thủ Thư" as Librarian
boundary "FormTraSach" as UI
control "MuonTraController" as Control
entity "ChiTietPhieuMuon" as Detail
entity "DauSach" as Book
entity "PhieuPhat" as Fine

Librarian -> UI: 1. Nhập MaPhieuMuon
UI -> Control: 2. LayThongTinMuon(MaPhieuMuon)
Control -> Detail: 3. Query danh sách sách đã mượn
Detail --> Control: 4. Trả về danh sách chi tiết mượn
Control --> UI: 5. Hiển thị danh sách sách cần trả

Librarian -> UI: 6. Chọn sách trả, nhập TinhTrangTra & Check quá hạn/mất
UI -> Control: 7. XacNhanTraSach(List<MaChiTiet>, NgayTra, LyDoPhat, PhiPhat)

loop Mỗi cuốn sách trả
    Control -> Detail: 8. Cập nhật NgayTraThucTe & TinhTrangTra
    Control -> Book: 9. TangSoLuongHienCo(+1)
end

alt Có vi phạm (Trả trễ / Rách / Mất sách)
    Control -> Fine: 10. TaoPhieuPhat(MaChiTiet, LyDo, PhiPhat)
    Fine --> Control: 11. Xác nhận tạo phiếu phạt
end

Control --> UI: 12. Hiển thị kết quả trả sách & Tổng phí phạt (nếu có)
@enduml
