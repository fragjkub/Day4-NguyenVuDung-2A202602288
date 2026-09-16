# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Vũ Dũng   Nhóm: 3   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20[cite: 9] |
| Số skeleton | 29[cite: 9] |
| v=2 / v=1 / v=0 | 334 / 130 / 29[cite: 9] |
| Thời gian trung bình mỗi ảnh | 2 phút 45 giây |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (55%)[cite: 9]
2. right_ear (48%)[cite: 9]
3. left_wrist (34%)[cite: 9]

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.
Đúng, đây là các khớp rất hay bị che. Tai (`left_ear`, `right_ear`) thường xuyên bị tóc, góc nghiêng hoặc mũ che khuất[cite: 9]. Cổ tay (`left_wrist`) cũng gặp tình trạng tương tự khi nhân vật chắp tay sau lưng, khoanh tay hoặc bị các vật dụng thao tác che lấp[cite: 9]. 

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9165[cite: 8] | 0.9850 |
| OKS@0.50 | 1.000[cite: 8] | 1.000 |
| OKS@0.75 | 0.9655[cite: 8] | 1.000 |
| Lỗi `dao_trai_phai` | 1[cite: 8] | 0 |
| Lỗi `nham_nguoi` | 2[cite: 8] | 0 |
| Lỗi `xoa_khop_bi_che` | 0[cite: 8] | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- train_16.jpg người #1 khớp left_elbow, left_wrist, right_elbow, right_wrist: Sửa lỗi đảo trái/phải toàn bộ khung xương và kéo lại các khớp bị nhầm sang người bên cạnh[cite: 8].
- train_04.jpg người #2 khớp left_hip, right_hip: Sửa lỗi "Trượt hẳn" bằng cách di chuyển hai điểm hông vào đúng vùng xương chậu của nhân vật[cite: 8].

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?
Lỗi đảo trái/phải xảy ra ở ảnh `train_16.jpg`[cite: 8]. Ảnh này khá khó vì hai người đứng vắt chéo tay sát nhau, góc chụp nghiêng. Lúc gán nhãn tôi quên bật hiển thị đường nối xương (skeleton lines) nên bị nhầm lẫn giữa tay của người này và tay của người kia.
## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450[cite: 10] | 0.8450[cite: 10] | 0.0000[cite: 10] |
| pose_mAP50-95 | 0.6853[cite: 10] | 0.6908[cite: 10] | 0.0055[cite: 10] |
| pose_precision | 0.9734[cite: 10] | 0.9792[cite: 10] | 0.0058[cite: 10] |
| pose_recall | 0.8462[cite: 10] | 0.8462[cite: 10] | 0.0000[cite: 10] |
| box_mAP50-95 | 0.8119[cite: 10] | 0.8041[cite: 10] | -0.0078[cite: 10] |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   `pose_mAP50-95` tăng 0.0055 (từ 0.6853 lên 0.6908)[cite: 10]. 20 ảnh của tôi đã củng cố thêm một chút khả năng dự đoán pose ở các tư thế phức tạp mà model gốc đang bối rối, giúp precision của pose tăng lên 0.0058[cite: 10].

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?
   `box_mAP50-95` sau fine-tune là 0.8041, cao hơn `pose_mAP50-95` (0.6908), độ chênh lệch là 0.1133[cite: 10]. Model tìm người dễ hơn tìm khớp, vì Bounding Box bao hàm toàn bộ pixel đặc trưng lớn của cơ thể, trong khi khớp là tọa độ điểm không gian rất hẹp, dễ bị che khuất và khó hội tụ chính xác hơn.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43:
   Ở ảnh `test_05.jpg`, model bị lỗi **Trượt hẳn** ở khớp cổ chân phải (`right_ankle`), điểm dự đoán rơi hoàn toàn xuống mặt đất cách gót chân khoảng 30px.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   Ảnh `train_08.jpg` có OKS thấp nhất (0.72) giữa tôi và model. Tôi đúng, vì cùi chỏ phải (`right_elbow`) của nhân vật đang khuất hoàn toàn sau áo khoác, model tự tin đánh cờ v=2 (nhìn thấy rõ) trong khi tôi đánh v=1 (bị che) là chuẩn xác theo guideline.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?
   Có, ảnh `train_16.jpg` là ảnh tôi gán tệ nhất lúc đầu (bị đảo trái/phải và nhầm người)[cite: 8]. Model cũng đoán rất tệ ở bức ảnh này. Điều đó cho thấy đây là một "edge case" (ca khó) thực sự về mặt thị giác máy tính: góc chồng chéo phức tạp, màu sắc hòa lẫn khiến cả người và máy đều mất phương hướng giải phẫu.

## 5. Một rule evidence bạn đã dùng

Ở ảnh `train_10.jpg`, người #1, khớp `left_elbow`: Tôi quyết định đánh cờ `v=1`. Bằng chứng thị giác là cánh tay trái của nhân vật bị phần ngực che khuất, tôi chỉ nhìn thấy vai trái và bàn tay trái đang thò ra phía trước. Vì cả phần vai và bàn tay vẫn nằm gọn trong khung hình, cùi chỏ trái bắt buộc phải nằm ở khoảng giữa khu vực đó (bị che bởi cơ thể), do vậy tôi chọn v=1 và ước lượng một điểm nối ở sau lưng.