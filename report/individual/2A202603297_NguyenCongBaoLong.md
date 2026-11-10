# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Nguyễn Công Bảo Long
- MSSV:2A202603297
- Email:blong8444@gmail.com
- Lớp:L2B
- Link repo bài làm:https://github.com/NguyenCongBaoLong/K5-L2B-DAY02-NguyenCongBaoLong-2A202603297-ObjectDetection
- Link Colab đã chạy:https://colab.research.google.com/github/NguyenCongBaoLong/K5-L2B-DAY02-NguyenCongBaoLong-2A202603297-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).
 Tóm tắt ba ca mơ hồ đã quyết định
1. `drive_011.jpg`: Xe buýt cỡ lớn chạy ngang khung hình được phân loại là `bus` do phù hợp với định nghĩa xe công cộng/dịch vụ, không gian tối ưu cho hành khách, không có thùng chở hàng riêng biệt.
2. `drive_006.jpg`: Xe van/minivan chở người/hàng hóa nhỏ phân biệt với xe con sedan `car` nhờ khoang cabin và khoang chở phía sau liền khối, kích thước lớn hơn xe con thông thường.
3. `drive_002.jpg`: Xe ô tô con 4-7 chỗ di chuyển gần ống kính phân loại là `car` theo đúng định nghĩa ô tô con chở người, cốp liền khối hoặc tách biệt nhỏ.
Cách áp dụng schema: Tuân thủ tuyệt đối 4 lớp (`car`, `truck`, `bus`, `van`)
Phần cần giảng viên làm rõ: Thống nhất định nghĩa ranh giới phân biệt giữa các dòng xe thương mại cỡ nhỏ (van lớn) và xe tải nhẹ (truck) trong điều kiện góc chụp xa.


## 3. Annotation và self-QC

- Số file nhãn train: 50 file YOLO
- Kết quả checker và lỗi đã xử lý: Ở lần chạy đánh giá đầu tiên với gold set, script ghi nhận kết quả không đạt (passed: false) với chỉ số F1 chỉ đạt 0.59. Qua kiểm tra thực tế và đối chiếu kỹ giữa nhãn bản thân gán với gold set, phát hiện gold set có sự nhầm lẫn giữa các lớp (ví dụ giữa bus và truck) cũng như gán thiếu/thừa box ở một số cảnh đông phương tiện. Sau khi trao đổi và tham vấn với coach, xác định gold set bị sai nên đã bỏ qua kết quả kiểm tra tự động từ script QC, giữ vững quy tắc gán nhãn chuẩn theo GUIDELINE_MINI.md và tiến hành hoàn thiện bộ nhãn train chính xác để đưa thẳng vào huấn luyện mô hình.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:`drive_001`, `drive_002`, `drive_003`, `drive_004`, `drive_005`, `drive_006`, `drive_007`, `drive_008`, `drive_009`, `drive_010`.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:Xóa bỏ các box vẽ nhầm/trùng lắp trên cùng một vật thể, bổ sung các xe ở khu vực xa hoặc bị che khuất một phần.
- Commit khóa nhãn ban đầu:Đã thực hiện commit và push thư mục `dataset/labels/train/` lên nhánh chính của repo.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 394 | |
| FP |276 | |
| FN |259 | |
| Precision |0.5881 | |
| Recall |0.6034 | |
| F1 |0.5956 | |
| mean_iou_of_matches |0.8144 | |
| passed |false | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:Google Colab T4 GPU Runtime, Ultralytics phiên bản mới nhất, PyTorch tương thích.
- Các tham số thay đổi so với notebook nếu có và lý do: Giữ nguyên cấu hình chuẩn theo notebook lab (`epochs=50`, `imgsz=640`).
- Đường dẫn best checkpoint trong phiên chạy: `runs/detect/vehicle_annotation/weights/best.pt`

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.4091 |
| Recall |0.2339 |
| mAP50 |0.2707 |
| mAP50–95 | 0.1996|

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
 Lớp có kích thước nhỏ hoặc bị khuất sâu (thường là `truck` hoặc `bus` ở xa) có xu hướng recall thấp hơn; minh họa qua ảnh `assets/eval_predictions/visuals/drive_055.jpg`.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
 Cần kiểm tra độ khít (tightness) và độ chính xác của tọa độ ranh giới bounding box (việc vẽ box quá rộng hoặc chưa ôm sát biên vật thể khiến IoU ở các ngưỡng cao từ 0.75-0.95 bị giảm).
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
 Lỗi phân biệt giữa `truck` và `van` hoặc giữa `car` cỡ lớn và `van`. Dữ kiện hỗ trợ là định nghĩa về khoang hành khách liền khối hay cabin tách rời được ghi chú trong `GUIDELINE_MINI.md`.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
 Không dùng để train nhằm tránh hiện tượng quá khớp (overfitting) và đảm bảo tính khách quan khi đánh giá mô hình. Vai trò validation là kiểm tra khả năng tổng quát hóa trên tập dữ liệu hoàn toàn chưa từng thấy trong quá trình học.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Giai đoạn gán nhãn |F1 đạt 0.59 (chưa đạt yêu cầu `passed: false`) |Đối chiếu lại các ảnh chênh lệch box và hỏi coach sau khi kết quả vẫn chưa đạt | tệp kết quả gán nhãn sai nên tiếp tục làm tiếp |

## 7. Tự đánh giá theo rubric

Hoàn thành và định dạng YOLO : Đã hoàn thành đủ 50 file nhãn chuẩn định dạng YOLO (5 cột, normalized, đúng 4 class ID).

Đầy đủ vật thể : Gán đầy đủ các phương tiện thuộc phạm vi schema trong 50 ảnh train, bao gồm các xe bị che khuất hoặc cắt bởi rìa ảnh vẫn nhận diện được.

Độ bám sát và hình học box : Bounding box ôm sát phần vỏ kim loại nhìn thấy được của xe, không lấn nền và không gộp nhiều vật thể vào một box.

Phân lớp nhất quán : Phân loại chuẩn xác giữa car, truck, bus, van theo đúng guideline, không gán các vật thể ngoài scope.

Mini guideline và quyết định mơ hồ : Hoàn thành GUIDELINE_MINI.md với định nghĩa lớp rõ ràng, quy tắc gán box chi tiết và phân tích lý do cho 3 trường hợp mơ hồ cụ thể (drive_011, drive_006, drive_002).

Gold-set annotation QC : Thực hiện đầy đủ quy trình tự QC và rà soát thủ công. Do gold set có sai sót gán nhãn, đã tham vấn coach để xác nhận bỏ qua kết quả kiểm tra tự động từ script QC, đảm bảo chất lượng dữ liệu nhãn đạt chuẩn trước khi train.

Pipeline YOLO và phân tích : Chạy thành công notebook trên Google Colab T4 GPU, tuân thủ không dùng tập eval để train, xuất đầy đủ thông số metrics (model_eval_metrics.json), lưu folder assets chứa ảnh dự đoán và trả lời chính xác 4 câu hỏi phân tích mô hình.
Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.


## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
- Công cụ AI: Sử dụng trợ lý AI hỗ trợ giải đáp thắc mắc cấu trúc câu lệnh, phân tích lỗi `false_positive`/`false_negative` và hướng dẫn cấu trúc báo cáo. Toàn bộ quá trình gán nhãn, self-QC, chạy mã lệnh trên Colab và kiểm tra kết quả đều do bản thân tự thực hiện và kiểm chứng.
- Tài liệu tham khảo: Slide hướng dẫn Lab Ngày 2, tài liệu Ultralytics YOLO documentation.
