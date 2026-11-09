# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Trần Đình Quang Vinh
- MSSV: 2A202603041
- Email: trandinhquangvinh0801@gmail.com
- Lớp: 2A
- Link repo bài làm: https://github.com/QuVinh123/K5-L2A-DAY02-TranDinhQuangVinh-2A202603041-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1FIq4DKaLlbGqoPzuuh8zxCblJGONGGRz#scrollTo=c4cb88bb

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Lớp thể hiện loại xe, không thể hiện trạng thái đỗ, di chuyển, bị che hoặc bị cắt khung. Schema gồm bốn lớp: car, truck, bus, van, phân loại theo loại xe, không theo trạng thái đỗ, di chuyển, bị che hoặc cắt khung. Không gán vật thể ngoài bốn lớp. Quy tắc vẽ box và ba ca mơ hồ được trình bày trong

## 3. Annotation và self-QC

- Số file nhãn train: 50 file YOLO, cùng basename với 50 ảnh train.
- Kết quả checker và lỗi đã xử lý: Đạt 50/50 file; bản hiện tại không có lỗi định dạng, tọa độ hoặc box vượt biên. Chưa có ghi chép lỗi đã sửa trước đó.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: Cần bổ sung danh sách bạn thực tế đã tự rà; trợ lý mới hỗ trợ xem drive_040, drive_041, drive_048.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Phát hiện ID lớp chưa khớp schema, chẳng hạn xe buýt ở drive_048. Cần xác nhận mapping với giảng viên trước khi sửa; chưa có kết quả xác nhận đã sửa nhãn.
- Commit khóa nhãn ban đầu: Cần bổ sung mã SHA của commit đã khóa nhãn trước khi nhận gold.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 536|- |
| FP | 118|- |
| FN | 117| -|
| Precision |0.8196 |- |
| Recall |0.8208|- |
| F1 |0.8202 |- |
| mean_iou_of_matches |0.7512 |- |
| passed |true |- |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:Tesla T4 (14913MiB)
- Các tham số thay đổi so với notebook nếu có và lý do: Không có sự thay đổi nào về tham số mặc định của bài Lab. Các cấu hình chuẩn bao gồm epochs=50, imgsz=640, seed=20260909, và model yolo26n.pt được giữ nguyên để đảm bảo tính nhất quán.
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.4245 |~42.5%|
| Recall |0.4157 |~41.6%
| mAP50 |0.4558 |~45.6%
| mAP50–95 |0.3287 |~32.9%


Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
- Trả lời: Lớp truck có Recall thấp nhất bằng 0.0 (mô hình không phát hiện được bất kỳ xe tải nào trong số 5 xe tải thực tế có trên các ảnh eval).
- Trong ảnh drive_053.jpg hoặc drive_058.jpg (những ảnh có chứa xe tải thực tế theo thống kê nhãn). Dù mô hình nhận diện được rất nhiều xe con (car), nó hoàn toàn bỏ qua hoặc nhận diện sai xe tải lớn thành xe con (car), dẫn đến recall của truck bằng 0.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
- Khi mAP50 cao nhưng mAP50-95 thấp, điều này phản ánh khung bao dự đoán (bounding box) lệch nhiều so với thực tế (Ground Truth) hoặc kích thước box không khớp chính xác (quá rộng hoặc quá hẹp).
- Ở ngưỡng IoU 0.5, mô hình tìm đúng vị trí chung chung của vật thể. Nhưng khi tăng ngưỡng IoU lên cao từ 0.5 đến 0.95, do viền box lệch trục, bo không sát mép vật thể hoặc bao cả các phần rìa thừa, độ trùng khớp (IoU) tụt xuống dưới ngưỡng yêu cầu và box bị coi là đoán sai.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Có. Các lỗi phổ biến thường liên quan chặt chẽ đến Guideline phân loại:
- Nhầm lẫn giữa van và car / truck: Một số dòng xe bán tải (pickup), xe SUV cỡ lớn hoặc xe tải nhỏ dễ bị người gán nhãn hoặc mô hình nhận diện không nhất quán (lúc gán là van, lúc là car).
- Khoảng cách và kích thước nhỏ: Guideline thường có quy định giới hạn kích thước tối thiểu (ví dụ: chỉ gán các xe có chiều rộng/chiều cao lớn hơn một số pixel nhất định). Việc mô hình cố gắng dự đoán hoặc người gán gán các xe quá xa ở hậu cảnh gây ra nhiễu thông tin học tập.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
- Việc giữ riêng 10 ảnh eval không đưa vào tập train là cực kỳ quan trọng vì:
- Tránh hiện tượng Overfitting (Quá khớp): Nếu dùng ảnh eval để train, mô hình sẽ "học thuộc lòng" các bức ảnh này. Khi đó, kết quả đánh giá sẽ cực kỳ cao nhưng không phản ánh đúng năng lực tổng quát hóa (generalization) thực tế của mô hình khi gặp dữ liệu hoàn toàn mới.
- Đảm bảo tính khách quan: Tập eval đóng vai trò như một thước đo độc lập để kiểm thử xem mô hình thực sự hiểu đặc trưng của các lớp xe hay chỉ đang ghi nhớ tập dữ liệu huấn luyện.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Xuất nhãn | YAML xuất đảo bus/truck so với schema gốc. |So sánh data.yaml gốc và YAML trong bản lab lồng. |Cần xác nhận mapping và đối chiếu nhãn; chưa xác nhận đã remap.|
|Gold QC|Một số ID của gold không khớp loại xe theo schema tài liệu.|Xem drive_040, drive_048 và đối chiếu dòng nhãn.|Cần giảng viên xác nhận; chưa sửa gold.|
|Diễn giải metric| | F1=0.8202 nhưng mean IoU=0.7512.|Đọc matching cùng lớp, tính lại metric và kiểm tra bỏ qua lớp để chẩn đoán.|Đã giải thích được sự khác biệt về đối tượng được tính trong hai chỉ số.|
|Cấu trúc dữ liệu|Có bản lab lồng và nhãn nằm sâu thêm một cấp.|Đọc cây thư mục và xác định bộ nhãn được dùng để tái tính JSON.|Cần bảo đảm lệnh local và Colab cùng dùng đúng bộ nhãn.|
|YOLO/Colab|Bản notebook có output lỗi clone cũ, chưa có kết quả train/eval.|Đọc source và output notebook hiện có.|Cần chạy lại và bổ sung kết quả thực tế.|

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.

- **Công cụ hỗ trợ:** ChatGPT/Codex.
- **Phạm vi hỗ trợ:** đọc tài liệu và mã kiểm tra, kiểm tra cấu trúc nhãn, tái tính metric, hỗ trợ đối chiếu ba ảnh, gợi ý cách diễn đạt guideline và soạn dự thảo báo cáo.
- **Phần học viên đã tự kiểm tra:** **CẦN BỔ SUNG trung thực** ảnh đã xem, quyết định lớp/box và các kết quả đã tự chạy.
- Không coi phần trợ lý hỗ trợ là bằng chứng học viên đã hoàn thành annotation, rà 10 ảnh hoặc train model.
- Tài liệu sử dụng: README.md, GUIDE.md, CHECKPOINTS.md, RULES.md, RUBRIC.md, SUBMISSION.md và hai script trong tools/ của bài lab.
- Số liệu annotation lấy từ JSON trong bản tải về và kết quả tái tính trên cùng bộ nhãn. Không bổ sung số liệu model giả định.
