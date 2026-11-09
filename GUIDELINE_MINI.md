# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe ô tô con, xe SUV, xe hơi taxi | |
| `truck` |xe có thùng chở hàng to ở sau | |
| `bus` |xe có nhiều chỗ ngồi, dài và to | |
| `van` |xe to hơn car nhưng phí sau để chở hàng | |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật nhỏ nhất bao quanh phần thân xe nhìn thấy được, hạn chế lấy thêm nền và không gộp nhiều xe vào một box.
- Vật thể bị che khuất: vẫn gán nhãn nếu phần nhìn thấy đủ để xác định lớp; box chỉ bao quanh phần nhìn thấy, không suy đoán phần bị che.
- Vật thể bị cắt bởi rìa ảnh: chỉ vẽ box quanh phần xe nằm trong ảnh, cạnh box chạm rìa ảnh; không mở rộng ra ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: gán nhãn nếu vẫn nhận diện chắc chắn được lớp. Nếu không xác định được, không tự đoán; ghi tên ảnh và tình huống vào guideline để hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_040` |Xe trắng dạng minivan ở giữa ảnh có thể nhầm với car hoặc bus. |Gán van — ID 4, vẽ box sát phần xe nhìn thấy. |Schema xếp minivan chở người hoặc hàng vào van. |
| `drive_041` |Xe buýt lớn ở tiền cảnh bị cắt bởi mép trái ảnh. |Gán bus — ID 2; box chỉ bao phần xe trong ảnh. |Xe bị cắt khung vẫn được gán nếu nhận diện được lớp; không đoán phần ngoài ảnh.|
| `drive_048` |Xe buýt khớp nối có hai phần thân nối liền, dễ bị tách thành hai đối tượng. |Gán một box bus — ID 2, bao quanh toàn bộ phần xe nhìn thấy.|Đây là một xe buýt hoàn chỉnh; mỗi xe tương ứng một box. |
