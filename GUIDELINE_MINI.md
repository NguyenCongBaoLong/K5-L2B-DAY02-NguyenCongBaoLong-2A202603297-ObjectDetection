# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |xe ô tô con chở người |ô tô 4-7 chỗ, cốp xe liền khối hoặc tách biệt nhỏ |
| `truck` |xe chở hàng hóa khối lượng lớn |chủ yếu là 2-3 chỗ ở cabin |
| `bus` |xe công cộng hoặc dịch vụ |không có thùng, không gian tối ưu cho ghế|
| `van` |xe chở cả người và hàng hóa nhỏ |từ 2-9 chỗ, khoang cabin và khoang hàng liền khối |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Ôm trọn phần thân vỏ kim loại nhìn thấy được của xe
- Vật thể bị che khuất: Vẫn gán box quanh phần diện tích nhìn thấy được của xe nếu xác định rõ lớp.
- Vật thể bị cắt bởi rìa ảnh: Box chạm đúng biên/rìa của bức ảnh tại điểm xe bị cắt; không tự phỏng đoán ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: Nếu vật thể quá nhỏ hoặc không nhìn rõ thuộc lớp nào, không tự đoán.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- | 
| `drive_002` |Xe ô tô con 4-7 chỗ di chuyển gần ống kính|Gán nhãn car |Đúng với định nghĩa ô tô con chở người, cốp xe liền khối hoặc tách biệt nhỏ, không thuộc diện xe tải hay xe buýt. |
| `drive_006` |Xe van/minivan chở người/hàng hóa nhỏ | Gán nhãn van| Khoang cabin và khoang chở phía sau liền khối, kích thước lớn hơn xe con thông thường nhưng chuyên chở hành khách/hàng nhỏ. |
| `drive_011` |Xe buýt cỡ lớn dài chạy ngang qua khung hình | Gán nhãn bus | Phù hợp với định nghĩa xe công cộng/dịch vụ, không gian tối ưu cho ghế, không có thùng chở hàng riêng biệt.|
