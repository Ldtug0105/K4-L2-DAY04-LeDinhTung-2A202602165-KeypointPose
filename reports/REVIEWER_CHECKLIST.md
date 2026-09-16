# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Bài trong workspace   Người kiểm: Lê Đình Tùng   Ngày: 2026-09-16

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels <bài của họ>
python3 tools/visualize_pose.py --images dataset/images/train --labels <bài của họ> --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare <bài của họ>
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | Cảnh báo | Có 28 skeleton; file đối chiếu có 29 và thêm 1 người ở train_13.jpg. |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | Đạt có điều kiện | Không có cảnh báo đảo trái/phải từ checker; visualize đã kiểm tra các ca nhiều người train_01, train_13, train_15. |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | Đạt có điều kiện | Checker không báo keypoint rơi vào box người khác; các ảnh đại diện không có đường nối sang người bên cạnh. |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | Cảnh báo | Checker có 18 cảnh báo; gold có 12 lỗi `xoa_khop_bi_che`. |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | Cảnh báo | Có 119 điểm v=0; cần rework các cảnh báo. |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | Chưa xác nhận hoàn toàn | JSON không có trường Hidden riêng; cần soi toàn bộ 20 ảnh bằng mắt để kết luận vị trí v=2. |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | Đạt | `person_keypoints_default.json` có 20 ảnh, 28 annotation; mỗi annotation có đúng 51 số. |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | Đạt | Checker đọc 20/20 file và không báo lỗi định dạng; `data.yaml` có `kpt_shape: [17, 3]`. |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | Đạt một phần | Đã có report của bài này và đối chiếu với file `_dd`; bảng so sánh ghi nhận lệch lớn ở left_hip và hai tai. |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | Đạt một phần | Đã ghi 3 ca từ gold; cần chèn screenshot CVAT thực tế. |
| 11 | `check_pose_labels.py` chạy 0 lỗi | Đạt | Có 18 cảnh báo nhưng 0 lỗi chặn nộp. |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_13.jpg | 1 | toàn bộ skeleton | Thiếu một người so với gold | Bổ sung người còn thiếu, đủ 17 điểm. |
| train_01.jpg | 1 | left_wrist | Dùng v=0 cho khớp bị che | Đặt chấm ước lượng và đổi thành v=1. |
| train_10.jpg | 1 | left_hip, right_hip | Dùng v=0 cho hai hông bị che | Đặt chấm theo trục thân/đùi và đổi thành v=1. |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Dùng v=0 thay cho v=1 ở khớp bị che hoặc khó quan sát; gold ghi nhận 12 lỗi `xoa_khop_bi_che` và checker có 18 cảnh báo liên quan.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Cả hai: cần thao tác rework, đồng thời guideline phải nhắc rõ “còn trong khung thì đặt chấm v=1”, không dùng v=0 vì không nhìn thấy bề mặt.
