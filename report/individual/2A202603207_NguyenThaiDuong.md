# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên
- Họ tên: Nguyễn Thái Dương
- MSSV: 2A202603207
- Lớp: K5 L2A
- Link repo bài làm: https://github.com/Duongbotroll/K5-L2A-Day02-NguyenThaiDuong-2A202603207-ObjectDetection
- Link Colab đã chạy: [dán link sau khi chạy xong]

## 2. Schema và mini guideline
Xem [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Ba ca mơ hồ (drive_001, drive_017, drive_043) đều là xe bị che/mờ nhưng vẫn nhận diện được lớp → vẫn gán box quanh phần nhìn thấy.

## 3. Annotation và self-QC
- Số file nhãn train: 50 (632 box)
- Checker: ĐẠT 50/50. Lỗi đã xử lý: lần export đầu từ CVAT nhầm định dạng "CVAT for images" (XML) nên 50 file nhãn rỗng; đã chuyển XML sang YOLO theo công thức trong GUIDE.md và kiểm tra lại.
- Ảnh đã rà bằng mắt: drive_001, drive_003, drive_005, drive_010, drive_017, drive_020, drive_025, drive_030, drive_043, drive_050.
- Commit khóa nhãn: f82d774

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
[điền sau khi Colab chạy xong]

4. Vì sao không dùng 10 ảnh eval để train? Vì eval dùng để đo khả năng tổng quát của model; nếu train trên đó thì metric bị đánh giá quá cao. Trong notebook, eval còn là validation để chọn best.pt nên không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| 10/11 sáng | Không vào được GitHub/CVAT (ERR_ADDRESS_UNREACHABLE) | Đổi mạng, thử lại | Mất ~30 phút, không kịp rework |
| Export nhãn | Export nhầm định dạng XML → nhãn rỗng | Chuyển XML sang YOLO bằng script | Đạt checker 50/50 |

## 7. Tự đánh giá theo rubric
Đã có: 50 nhãn đúng định dạng, guideline + 3 ca mơ hồ, gold QC (IoU đạt, F1 chưa đạt, chưa rework). Pipeline YOLO: [cập nhật].

## 8. Công cụ AI và tài liệu tham khảo
Dùng Claude để hỏi quy trình, cách dùng CVAT/Codespaces/Colab, viết script chuyển định dạng XML→YOLO và các lệnh git. Nhãn do tôi tự vẽ trên CVAT; số liệu lấy từ kết quả chạy thật.