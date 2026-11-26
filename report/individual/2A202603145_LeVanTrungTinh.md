# Báo cáo Day 14 — Lê Văn Trung Tính — 2A202603145

- Mã học viên dùng với `--student`: 2A202603145
- `seed_tag` trong `frames_A.txt`: 393590736f

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: `score_thr` (tăng lên 0.3 - 0.4) kết hợp với `min_points` (tăng lên 5 hoặc 10).
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Quan sát ảnh BEV và phân bố box tập A (`s01f36`, `s03f56`, `s05f16`), số lượng box thừa (False Positive) chiếm ưu thế lớn, đặc biệt là các box nhỏ lớp `Cyclist` (frame `s01f36` có tới 25 box `Cyclist`) và các box ở cự ly xa ven đường có rất ít điểm phản xạ. Do đó việc tăng `score_thr` để loại bỏ các dự đoán có score thấp và đặt `min_points >= 5` để loại bỏ box rác ít điểm sẽ triệt tiêu được lượng lớn box thừa mà không làm sót thêm nhiều xe thật. Mặt khác, PointPillars KITTI không hỗ trợ nhận diện `Bus` và `Truck` nên các xe lớn bị sót vẫn phải gán thêm thủ công.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_2A202603145/compare/s01f36.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_2A202603145/frames_B.txt --config d14_2A202603145/best_config.json --gt d14_2A202603145/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật):
  1. Tập tune A chỉ có 3 frame nên việc chọn siêu tham số dễ gặp hiện tượng overfitting trên một số bối cảnh cụ thể của sequence.
  2. Model PointPillars huấn luyện trên KITTI chỉ phát hiện 3 lớp (Car, Cyclist, Pedestrian), hoàn toàn không nhận diện được xe tải (Truck) và xe buýt (Bus) vốn xuất hiện nhiều trong giao thông thực tế tại Việt Nam.
  3. Chi phí sửa dựa trên công thức đại diện (trọng số xóa 0.3, thêm 1.0, sửa 0.5) chưa phản ánh chính xác 100% thời gian thao tác tay thực tế của annotator trên giao diện 3D CVAT.
