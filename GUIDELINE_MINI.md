# Mini guideline - nhóm: ___3___  |  người gán: Nguyễn Vũ Dũng  |  ngày: 16/9/2026

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
| Hông của người mặc quần áo dài | Đánh `v=1`, dóng từ vai và đùi để ước lượng vị trí hông. | Khớp hông bị vải che nhưng vẫn nằm trong cơ thể ở trong ảnh. *(Xem ảnh mẫu: train_04.jpg)* |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Nếu thấy <50% tai: `v=1`. Nếu thấy >50%: `v=2`. Không dùng `v=0`. | Tai vẫn ở trên đầu nhân vật, chỉ bị che khuất (Occluded). *(Xem ảnh mẫu: train_03.jpg)* |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp từ đầu gối, mắt cá xuống đánh `v=0`, không đặt chấm. | Khớp đã thực sự nằm ngoài mép (Outside) của bức ảnh. *(Xem ảnh mẫu: train_01.jpg)* |
| Cổ tay nằm sau tay lái / sau thân mình | Đánh `v=1`, đặt chấm ở phía sau thân mình/tay lái. | Tay không thể biến mất, chỉ bị che. Cần đặt chấm để giữ dáng pose. *(Xem ảnh mẫu: train_08.jpg)* |
| Hai người chồng lên nhau | Khớp người sau bị người trước đè -> đánh `v=1`. | Bắt buộc bật hiển thị nối xương để không gán nhầm tay người này sang người kia. *(Xem ảnh mẫu: train_16.jpg)* |
| Người nhỏ đến mức nào thì không gán nữa | Bounding box (khung chữ nhật) có chiều cao < 40 pixel thì bỏ qua toàn bộ. | Hình quá nhỏ không thể thấy được đặc trưng tứ chi, dễ sinh ra nhiễu cho model. *(Xem ảnh mẫu: train_20.jpg)* |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_16.jpg`, người thứ `1`, khớp `left_elbow`, `left_wrist`

- Mơ hồ ở chỗ nào: Góc chụp nghiêng, hai người đứng sát nhau vắt chéo tay, rất khó phân biệt đâu là cùi chỏ/cổ tay trái của người thứ 1 và tay của người thứ 2[cite: 8].
- Bạn quyết thế nào: Bật hiển thị bộ xương (skeleton lines), dò dọc từ vai trái của người 1 xuống để đặt chấm, đánh `v=1` nếu bị tay người kia đè lên.
- Vì sao: Dò từ gốc vai xuống sẽ đảm bảo đúng mạch cơ thể học, không bị "nhảy" sang người bên cạnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Gây ra lỗi Nhầm người và Đảo trái/phải[cite: 8]. Model sẽ học sai toàn bộ cấu trúc tay và bị phạt gấp đôi khi augmentation lật ảnh ngang.

### Ca 2 - ảnh `train_04.jpg`, người thứ `2`, khớp `left_hip`, `right_hip`

- Mơ hồ ở chỗ nào: Nhân vật mặc áo khoác dài che hoàn toàn phần thân dưới, không có nếp gấp quần áo nào để căn cứ vị trí xương chậu, rất dễ trượt hẳn[cite: 8].
- Bạn quyết thế nào: Đánh cờ `v=1` (bị che), lấy giao điểm dóng thẳng từ eo xuống và từ đùi lên để chia tỷ lệ đặt chấm.
- Vì sao: Dù không nhìn thấy, hông vẫn ở đó. Khớp còn trong khung hình thì không được phép dùng `v=0`[cite: 12].
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu gán bừa sẽ dính lỗi "Trượt hẳn" rất nặng (ví dụ lệch > 200px)[cite: 8]. Model sẽ gán hông rơi ra ngoài cơ thể hoặc sai lệch tỷ lệ lưng/chân.

### Ca 3 - ảnh `train_10.jpg`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Cổ tay trái bị khuất hoàn toàn sau lớp áo, chỉ thấy một mẩu ngón tay thò ra[cite: 8].
- Bạn quyết thế nào: Đánh cờ `v=1`, đặt chấm ở vị trí ngay sát ngón tay bị khuất sau mép áo.
- Vì sao: Vì bàn tay vẫn còn trong khung hình, nên khớp cổ tay chắc chắn nằm ngay sau lớp vải đó.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu dùng `v=0` (outside), model sẽ học đặc trưng sai rằng nhân vật này bị cụt tay (mất phần cổ tay dù vẫn thấy bàn tay).

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `55%`[cite: 9] / họ `30%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Do guideline chưa rõ ràng ở các trường hợp tóc lòa xòa che một phần dái tai. Tôi có xu hướng hễ thấy vướng tóc là đánh `v=1`, trong khi bạn cùng nhóm cho rằng nhìn thấy được hình dáng tai thì đánh `v=2`.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Nếu tai bị tóc hoặc mũ che nhưng vẫn thấy rõ cấu trúc giải phẫu lớn hơn 50%, đánh `v=2` (Visible). Nếu bị che gần hết chỉ thấy lấp ló, đánh `v=1` (Occluded). Tuyệt đối không đánh `v=0`.