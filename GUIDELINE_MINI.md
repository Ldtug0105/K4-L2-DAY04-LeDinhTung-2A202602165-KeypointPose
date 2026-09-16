# Mini guideline - nhóm: VinAI  |  người gán: Lê Đình Tùng  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Xác định theo trục thân và phần tiếp nối của đùi; nếu hông bị vải che nhưng còn trong khung thì đặt chấm ước lượng, `v=1`. | Bám đúng luật lớp, tránh xoá hông vì không thấy bề mặt. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu còn xác định được vị trí tai từ đầu và bên đối diện thì đặt chấm, `v=1`; chỉ `v=0` khi tai ra ngoài mép ảnh. | Phân biệt che khuất với ra ngoài ảnh. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp phía trong khung vẫn gán đủ; khớp nằm ngoài mép dùng `v=0` và tọa độ 0. | Không xoá cả skeleton chỉ vì người bị cắt. |
| Cổ tay nằm sau tay lái / sau thân mình | Đặt tại vị trí giải phẫu ước lượng dựa trên cẳng tay và bàn tay liền kề, `v=1` nếu còn trong khung. | Checker đã cảnh báo nhiều trường hợp dùng v=0 cho khớp bị che. |
| Hai người chồng lên nhau | Gán theo từng cơ thể; dùng đầu, vai, hông và hướng các chi để giữ đúng người, không nối sang cơ thể bên cạnh. | Tránh lỗi nhầm người. |
| Người nhỏ đến mức nào thì không gán nữa | Không bỏ người chỉ vì nhỏ; nếu là người trong ảnh thì vẫn gán đủ 17 điểm theo khả năng quan sát và visibility. | Luật lớp yêu cầu mọi người trong ảnh đều có skeleton. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01.jpg`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Cổ tay bị che hoặc khó nhìn rõ nhưng người vẫn nằm trong khung.
- Bạn quyết thế nào: Đặt chấm theo hướng cẳng tay và chọn `v=1`.
- Vì sao: Kết quả gold báo bài hiện tại dùng `v=0` trong khi gold có `v=1` (`xoa_khop_bi_che`).
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học rằng cổ tay bị che là không tồn tại.

### Ca 2 - ảnh `train_10.jpg`, người thứ `1`, khớp `left_hip/right_hip`

- Mơ hồ ở chỗ nào: Quần áo che vùng hông nên khó xác định điểm bề mặt.
- Bạn quyết thế nào: Suy ra từ trục thân và hai đoạn đùi, đặt cả hai hông và dùng `v=1`.
- Vì sao: Gold đánh dấu cả hai khớp là bị che; bài hiện tại đã xoá chúng (`xoa_khop_bi_che`).
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học hình dáng thân-người bị khuyết tại vùng hông.

### Ca 3 - ảnh `train_14.jpg`, người thứ `2`, khớp `nose/left_eye/right_eye/left_ear`

- Mơ hồ ở chỗ nào: Khuôn mặt của người thứ hai khó quan sát rõ.
- Bạn quyết thế nào: Vẫn gán các điểm còn trong khung theo cấu trúc khuôn mặt; không để `v=0` chỉ vì khó thấy.
- Vì sao: Gold có các điểm này với `v=2`, còn bài hiện tại bị ghi `thieu_khop`.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học rằng mặt người ở tư thế/độ che này thiếu mắt, mũi và tai.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_hip` (bạn 10,7% / bạn cùng nhóm 41,4%, lệch 30,7 điểm phần trăm). Hai tai cùng lệch 24,1 điểm phần trăm (bạn 0% / bạn cùng nhóm 24,1%).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Có dấu hiệu guideline về khớp bị che chưa được áp dụng thống nhất, đặc biệt ở hông và tai; cần xem ảnh mẫu để quyết định từng điểm, không kết luận chỉ từ tỷ lệ.
- Luật mới bổ sung vào mục 2 sau khi thống nhất:

	Nếu box của người vẫn nằm trong ảnh và phần chi không nhìn thấy do người/vật khác che, phải đặt điểm ước lượng và chọn `v=1`; chỉ chọn `v=0` khi điểm nằm ngoài mép ảnh.
