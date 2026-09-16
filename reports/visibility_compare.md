# Visibility comparison

- Bài của tôi: `annotations/coco_keypoints/person_keypoints_default.json` - 20 ảnh, 28 skeleton.
- Bài bạn cùng nhóm: `annotations/coco_keypoints/person_keypoints_default_dd.json` - 20 ảnh, 29 skeleton.
- Khác biệt số người: `train_13.jpg` có 2 skeleton trong bài của tôi và 3 skeleton trong bài bạn cùng nhóm.

| # | Khớp | %v=1 của tôi | %v=1 bạn cùng nhóm | Lệch tuyệt đối |
| ---: | --- | ---: | ---: | ---: |
| 11 | left_hip | 10.7% | 41.4% | 30.7 điểm % |
| 3 | left_ear | 0.0% | 24.1% | 24.1 điểm % |
| 4 | right_ear | 0.0% | 24.1% | 24.1 điểm % |
| 12 | right_hip | 14.3% | 31.0% | 16.7 điểm % |
| 14 | right_knee | 7.1% | 20.7% | 13.5 điểm % |
| 13 | left_knee | 7.1% | 20.7% | 13.5 điểm % |
| 16 | right_ankle | 7.1% | 20.7% | 13.5 điểm % |
| 15 | left_ankle | 3.6% | 13.8% | 10.2 điểm % |
| 8 | right_elbow | 7.1% | 13.8% | 6.7 điểm % |
| 7 | left_elbow | 7.1% | 13.8% | 6.7 điểm % |
| 0 | nose | 3.6% | 6.9% | 3.3 điểm % |
| 1 | left_eye | 3.6% | 6.9% | 3.3 điểm % |
| 2 | right_eye | 3.6% | 6.9% | 3.3 điểm % |
| 6 | right_shoulder | 3.6% | 6.9% | 3.3 điểm % |
| 9 | left_wrist | 7.1% | 10.3% | 3.2 điểm % |
| 5 | left_shoulder | 14.3% | 17.2% | 3.0 điểm % |
| 10 | right_wrist | 17.9% | 20.7% | 2.8 điểm % |

## Nhận xét

Chênh lệch lớn nhất là `left_hip`, tiếp theo là hai tai. Đây là dấu hiệu cách xử lý khớp bị che chưa thống nhất, không đủ để kết luận một bên gán sai chỉ bằng tỷ lệ. Theo guideline của lớp, nếu khớp còn trong khung nhưng bị che thì vẫn đặt chấm ước lượng và chọn `v=1`; chỉ dùng `v=0` khi khớp thật sự nằm ngoài mép ảnh.
