# Review partner

Người gán được kiểm: bài trong workspace hiện tại  
Người kiểm: Lê Đình Tùng  
Ngày: 16/09/2026

Nguồn đối chiếu: `annotations/coco_keypoints/person_keypoints_default_dd.json` của bạn cùng nhóm có 29 skeleton; bài được kiểm có 28 skeleton.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_13.jpg | 1 | toàn bộ skeleton | Bài được kiểm thiếu một người so với file đối chiếu | Bổ sung người còn thiếu, đủ 17 keypoint. |
| train_01.jpg | 1 | left_wrist | Dùng `v=0` cho khớp bị che trong khi khớp còn trong ảnh | Đặt chấm theo hướng cẳng tay và đổi visibility thành `v=1`. |
| train_10.jpg | 1 | left_hip, right_hip | Dùng `v=0` cho hai hông bị che | Suy ra vị trí theo trục thân và hai đoạn đùi, đặt chấm và dùng `v=1`. |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất: dùng `v=0` thay cho `v=1` ở khớp bị che hoặc khó quan sát.
- Đây vừa là lỗi thao tác vừa cho thấy guideline chưa được áp dụng thống nhất. Quy tắc cần dùng là: nếu khớp còn trong khung nhưng bị che, vẫn đặt chấm ước lượng và chọn `v=1`; chỉ chọn `v=0` khi khớp thật sự nằm ngoài mép ảnh.

## Ghi chú kiểm tra

- `check_pose_labels.py`: 0 lỗi định dạng, còn 18 cảnh báo visibility.
- Có 12 lỗi `xoa_khop_bi_che` trong kết quả chấm với gold.
- Chưa có screenshot CVAT kèm theo nên các mục kiểm tra bằng mắt cần được xác nhận bổ sung trước khi nộp chính thức.
