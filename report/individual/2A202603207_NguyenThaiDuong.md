# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên
- Họ tên: Nguyễn Thái Dương
- MSSV: 2A202603207
- Lớp: K5 L2A
- Link repo bài làm: https://github.com/Duongbotroll/K5-L2A-Day02-NguyenThaiDuong-2A202603207-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1EW3NzRQyJoCbYD6fm8zjqFlSTfZBWNhj?usp=sharing

## 2. Schema và mini guideline
Xem [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Ba ca mơ hồ (drive_001, drive_017, drive_043) đều là xe bị che/mờ nhưng vẫn nhận diện được lớp → vẫn gán box quanh phần nhìn thấy.

## 3. Annotation và self-QC
- Số file nhãn train: 50 (632 box)
- Checker: ĐẠT 50/50. Lỗi đã xử lý: lần export đầu từ CVAT nhầm định dạng "CVAT for images" (XML) nên 50 file nhãn rỗng; đã chuyển XML sang YOLO theo công thức trong GUIDE.md và kiểm tra lại.
- Ảnh đã rà bằng mắt: drive_001, drive_003, drive_005, drive_010, drive_017, drive_020, drive_025, drive_030, drive_043, drive_050.
- Commit khóa nhãn: 98c68dc (do lỗi export, 50 file nhãn rỗng). f82d774 chỉ chuyển định dạng từ chính file CVAT đã export trước khi nhận gold (XML → YOLO), không sửa box nào. Lần chấm gold đầu (trên nhãn rỗng) cho TP=0, FN=653.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 375 | chưa rework |
| FP | 257 | |
| FN | 278 | |
| Precision | 0.5934 | |
| Recall | 0.5743 | |
| F1 | 0.5837 | |
| mean_iou_of_matches | 0.8115 | |
| passed | false | |

Kết quả ở `outputs/annotation_evaluation.json`. Mean IoU 0.81 cho thấy box bám sát; F1 thấp do nhiều box không ghép được (FP/FN). Nguyên nhân cần kiểm tra: sai lớp, bỏ sót xe nhỏ/xa, vẽ thêm xe gold không gán. Ngoài ra nhãn eval do giảng viên cung cấp có vẻ dùng class ID khác schema (xe buýt mang ID 3, minivan ID 1), có thể ảnh hưởng matching theo lớp — đã báo giảng viên. Chưa kịp rework trước hạn do sự cố mạng (mục 6).

## 5. Kết quả YOLO
- Runtime: Google Colab, CPU (GPU_mem 0G — không được cấp GPU), ~40s/epoch, 50 epoch. Ultralytics/Torch: Ultralytics 8.4.208 🚀 Python-3.13.18 torch-2.14.1+cpu CPU (Intel Xeon CPU @ 2.20GHz)
- Tham số: giữ nguyên notebook (yolo26n.pt, epochs=50, imgsz=640, seed=20260909).
- Best checkpoint: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.3902 |
| Recall | 0.2805 |
| mAP50 | 0.2764 |
| mAP50–95 | 0.2048 |
**Metrics theo lớp:**

| Lớp | Instances | Recall (R) |
|---|---:|---:|
| car | 94 | 0.755 |
| truck | 5 | 0.200 |
| bus | 2 | 0.000 |
| van | 12 | 0.167 |

**Trả lời câu hỏi cuối notebook:**

1. Lớp có recall thấp nhất là `bus` (R = 0.000, chỉ có 2 mẫu trong eval).
2. Với lớp `car`, mAP50 = 0.737 nhưng mAP50–95 = 0.518.
3. Lớp `van` có 12 mẫu trong eval nhưng mAP50 chỉ đạt 0.079.
Ảnh dự đoán: [assets/eval_predictions/visuals/](assets/eval_predictions/visuals/) — ví dụ [drive_051](assets/eval_predictions/visuals/drive_051.jpg). Log train và confusion matrix: [assets/yolo26_runs/](assets/yolo26_runs/).

4. Vì sao không dùng 10 ảnh eval để train? Vì eval dùng để đo khả năng tổng quát của model; nếu train trên đó thì metric bị đánh giá quá cao. Trong notebook, eval còn là validation để chọn best.pt nên không phải test set độc lập.

## 6. Khó khăn và cách xử lý
| Hôm qua máy em chưa cài được Ubuntu nên em dùng tạm Cvat online để label.
| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| 10/11 sáng | Không vào được GitHub/CVAT (ERR_ADDRESS_UNREACHABLE) | Đổi mạng, thử lại | Mất ~30 phút, không kịp rework |
| Export nhãn | Export nhầm định dạng XML → nhãn rỗng | Chuyển XML sang YOLO bằng script | Đạt checker 50/50 |
| 10/11 ~11:20–12:08 | Colab không cấp GPU, train CPU ~40s/epoch | Giữ nguyên để không mất tiến độ | Train xong ~12:05, push kết quả Colab sau deadline; đã báo giảng viên |

## 7. Tự đánh giá theo rubric
Đã có: 50 nhãn đúng định dạng, guideline + 3 ca mơ hồ, gold QC (IoU đạt, F1 chưa đạt, chưa rework). 
Pipeline YOLO: Colab chạy hết, giữ đúng split (không dùng eval để train), có metrics và 10 ảnh dự đoán trong assets/; kết quả Colab push sau 11:59.

## 8. Công cụ AI và tài liệu tham khảo
Dùng Claude để hỏi quy trình, cách dùng CVAT/Codespaces/Colab, viết script chuyển định dạng XML→YOLO và các lệnh git. Nhãn do tôi tự vẽ trên CVAT; số liệu lấy từ kết quả chạy thật.
