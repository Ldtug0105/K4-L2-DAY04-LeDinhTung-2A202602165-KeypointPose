# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Đình Tùng,  Nhóm: Trấn áp thế hệ trẻ, Ngày: 16/09/2026


## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 324 / 33 / 119 |
| Thời gian trung bình mỗi ảnh | Chưa có dữ liệu thời gian |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. right_wrist (18% v=1)
2. left_shoulder (14% v=1)
3. right_hip (14% v=1)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

Theo số liệu, cổ tay phải và vùng vai/hông là các vị trí hay bị che nhất, không đồng nghĩa mọi lần đều khó xác định giải phẫu. Cổ tay phải có 5/28 trường hợp `v=1`, còn vai trái và hông phải có 4/28; các ca này cần suy ra từ đoạn chi liền kề khi bề mặt bị che. Tai có 0% `v=1` nhưng nhiều `v=0`, cho thấy tai thường bị coi là ra ngoài/không gán hơn là được đánh dấu bị che và cần xem lại bằng mắt.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.8763 | Chưa có lần chạy sau rework |
| OKS@0.50 | 0.9655 | Chưa có lần chạy sau rework |
| OKS@0.75 | 0.8621 | Chưa có lần chạy sau rework |
| Lỗi `dao_trai_phai` | 0 | Chưa có lần chạy sau rework |
| Lỗi `nham_nguoi` | 0 | Chưa có lần chạy sau rework |
| Lỗi `xoa_khop_bi_che` | 12 | Chưa có lần chạy sau rework |

**Tôi đã sửa gì giữa hai lần chạy**:

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- Chưa có bản ghi lần chạy sau rework trong repo; chưa thể khẳng định thao tác nào đã sửa.
- Ưu tiên rework: train_01.jpg, người 1, left_wrist -> đặt chấm và dùng v=1 khi khớp bị che.
- Ưu tiên rework: train_10.jpg, người 1, left_hip và right_hip -> đặt chấm và dùng v=1 khi khớp còn trong khung.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong lần chấm hiện có trên toàn bộ 20 ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: người gán trong file `person_keypoints_default_dd.json`

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_hip | 10.7% | 41.4% | 30.7 điểm % | Guideline về hông bị che chưa thống nhất; cần đặt điểm và dùng v=1 khi hông còn trong khung. |
| left_ear | 0.0% | 24.1% | 24.1 điểm % | Khác biệt cách xử lý tai bị che, cần kiểm tra lại v=1/v=0 bằng ảnh. |
| right_ear | 0.0% | 24.1% | 24.1 điểm % | Khác biệt cách xử lý tai bị che, cần kiểm tra lại v=1/v=0 bằng ảnh. |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Khi người còn nằm trong khung, khớp bị che phải đặt chấm ước lượng và chọn `v=1`; chỉ chọn `v=0` khi khớp thật sự ra ngoài mép ảnh. Riêng hông bị quần áo che vẫn suy ra theo trục thân và hai đoạn đùi.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   Chỉ số này tăng 0.0055, từ 0.6853 lên 0.6908; vì vậy giả thuyết “giảm” không áp dụng cho kết quả hiện có. Tập 20 ảnh cải thiện rất nhẹ định vị pose trên test, trong khi box mAP50-95 giảm 0.0078, nên chưa có bằng chứng về một kiến thức đặc thù nào ngoài đánh đổi nhỏ ở định vị box.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Sau fine-tune, box mAP50-95 là 0.8041 và pose mAP50-95 là 0.6908, chênh 0.1133. Model tìm người dễ hơn tìm chính xác khớp, vì box chỉ cần bao đúng người còn pose phải đặt đúng từng điểm.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   `eval_model.json` không lưu lỗi theo từng ảnh hoặc ảnh visualize, nên chưa thể gọi tên một ảnh model đoán sai mà không tự đoán.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Kết quả hiện có chỉ chấm nhãn với gold: train_13.jpg, gold person 1, có OKS 0.0000 vì thiếu hẳn một người. Gold là căn cứ cho kết luận này; chưa có OKS per-image của model để so ai đúng.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   Chưa thể so sánh. Nhãn tệ nhất theo gold là train_13.jpg, nhưng `eval_model.json` không chứa bảng lỗi theo ảnh; cần output per-image từ notebook.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

Ví dụ evidence: train_01.jpg, người 1, left_wrist. Cổ tay bị che nhưng cẳng tay và hướng bàn tay vẫn cho căn cứ để ước lượng vị trí. Vì người còn trong khung, điểm phải được đặt và chọn `v=1`, không dùng `v=0`. Kết quả gold cũng phân loại ca hiện tại là `xoa_khop_bi_che`, nên đây là điểm cần rework.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
