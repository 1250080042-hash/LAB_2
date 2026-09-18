# SƠ ĐỒ THIẾT KẾ UML CHO CÁC FORM GIAO DIỆN (LAB 2)

## 1. Sequence Diagram: Luồng xử lý Form Mượn Sách

```plantuml
@startuml
autonumber
actor "Thủ Thư" as Librarian
boundary "FormMuonSach" as UI
control "MuonTraController" as Control
entity "TheDocGia" as Card
entity "DauSach" as Book
entity "PhieuMuon" as Slip

Librarian -> UI: 1. Nhập MaDocGia & Mã Sách
UI -> Control: 2. MuonSachRequest(MaDocGia, List<MaDauSach>)
Control -> Card: 3. KiemTraThongTinThe(MaDocGia)
Card --> Control: 4. Trả về trạng thái thẻ

alt Thẻ hợp lệ
    Control -> Book: 5. KiemTraSoLuongHienCo(MaDauSach)
    Book --> Control: 6. Trả về số lượng
    Control -> Slip: 7. TaoPhieuMuon()
    Slip --> Control: 8. Xác nhận tạo thành công
    Control --> UI: 9. Hiển thị thông báo thành công
end
@enduml
