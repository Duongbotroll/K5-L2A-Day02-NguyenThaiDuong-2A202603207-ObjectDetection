# Mini guideline — bài làm cá nhân

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người: sedan, SUV, taxi, bán tải | Bán tải vẫn là car, không phải truck |
| `truck` | Xe chở hàng có thùng/cabin lớn, đầu kéo | Thùng hàng cao hơn cabin; khác van ở phần thùng tách rời |
| `bus` | Xe buýt, minibus chở khách nhiều hàng ghế | Thân dài, nhiều cửa sổ dọc thân |
| `van` | Xe van/minivan một khối liền, chở hàng hoặc người | Cao hơn car, ngắn hơn bus, không có thùng tách rời |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: 4 cạnh chạm mép thân xe nhìn thấy, không lấn mặt đường/xe bên cạnh.
- Vật thể bị che khuất: vẽ một box từ điểm thấy được xa nhất bên này đến bên kia; không đoán phần bị che ở ngoài cùng.
- Vật thể bị cắt bởi rìa ảnh: box chạm đúng mép ảnh.
- Vật thể rất nhỏ/mơ hồ: không đoán lớp; bỏ qua nếu không nhận ra loại xe.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001` | Xe trắng bên phải bị cột đèn che một phần thân | Vẽ 1 box ôm trọn xe, gán `car` | Bị che nhưng vẫn nhận diện được lớp → vẫn gán, box quanh phần nhìn thấy |
| `drive_017` |Xe trắng bị mờ | Vẽ 1 box ôm trọn xe, gán `car`|  Bị mờ nhưng vẫn nhận diện được lớp → vẫn gán, box quanh phần nhìn thấy|
| `drive_043` |Xe ở góc dưới bị mờ và khuất|Vẽ 1 box quanh nửa xe, gán `car` | Bị mờ nhưng vẫn nhận diện được lớp → vẫn gán, box quanh phần nhìn thấy|