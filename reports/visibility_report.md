# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 12.75 khớp có v > 0 mỗi người
- Tổng: v=2 324 | v=1 33 | v=0 119

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 20 | 1 | 7 | 4% |
| 1 | left_eye | 17 | 1 | 10 | 4% |
| 2 | right_eye | 18 | 1 | 9 | 4% |
| 3 | left_ear | 11 | 0 | 17 | 0% |
| 4 | right_ear | 15 | 0 | 13 | 0% |
| 5 | left_shoulder | 24 | 4 | 0 | 14% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 23 | 2 | 3 | 7% |
| 8 | right_elbow | 24 | 2 | 2 | 7% |
| 9 | left_wrist | 20 | 2 | 6 | 7% |
| 10 | right_wrist | 20 | 5 | 3 | 18% |
| 11 | left_hip | 21 | 3 | 4 | 11% |
| 12 | right_hip | 21 | 4 | 3 | 14% |
| 13 | left_knee | 15 | 2 | 11 | 7% |
| 14 | right_knee | 17 | 2 | 9 | 7% |
| 15 | left_ankle | 16 | 1 | 11 | 4% |
| 16 | right_ankle | 15 | 2 | 11 | 7% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
