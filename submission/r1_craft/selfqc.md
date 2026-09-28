# Tự soát

- Tên task thiếu raw_fisheye → **đã xử lý**: task tên `Day11 · ADASIND · B4-dense · raw_fisheye`; cảnh báo do export từ *job* (XML job không chứa tên task). Bản khóa (mã 89B2-573F) vẫn là export job nên cảnh báo còn hiện; tên task trong CVAT đã đúng, không ảnh hưởng nhãn nên không khóa lại. Lần sau export từ trang Task (Actions → Export task dataset).

## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ — 21 box đều H≥40 (thấp nhất: 258420 Car H=41, Bike xa H=47). Vật <40 px ở xa (xe nhỏ sau auto) không box. Ca nghi ngờ đã quyết GIỮ: 258420 Truck #4 (khối xe tối bị che, occluded) và 270517 Bike #6 (vật xám cạnh cột điện, đọc là xe máy đậu) — sẽ đối chiếu reference ở P4.
- [x] lens_border và ego_body — 2 lens_border/frame giữ từ prefill, soát khớp vòng kính; 1 ego_body/frame ở góc dưới trái (tay áo caro, chân, sàn xe hai bánh); slice không chứa 006840/271039 nên cả 3 frame đều có ego.
- [x] Class sáu nhãn — auto-rickshaw = ThreeWheeler (6 box), xe con bạc 270517 là Car (hình dáng dễ nhầm với auto khi bị người đi bộ che); không có Bus.
- [x] Rider và Bike — 3 xe máy có người lái ở 258420 là 1 Bike mỗi xe (gộp người lái + xe theo R03); người lái xe ego không box (thuộc ego_body); người lái trong auto 270517 #1 không box riêng.
- [x] Geometry trên ảnh fisheye gốc — box bám phần thấy trên ảnh gốc; 270517 Bike #7 cắt ở mép phải chỉ bao phần thấy; không box nào phủ cả dãy xe.
- [x] truncated và occluded — truncated khớp hình học vòng kính/khung (chỉ 270517 Bike #7 = true); occluded: 258420 Truck #4, 270517 Car #4 (bị người đi bộ che), 310008 Pedestrian #5 (bị người trước che).
- [x] Vật thiếu hoặc box trùng — kiểm cặp cùng class IoU>0.5: không trùng; đã thêm xe máy xa 258420 #3 do model bỏ sót. Mép trái 270517 có một vật tối rất hẹp bị ảnh cắt, không nhận ra class → không box, ghi làm ca nghi ngờ.
- [x] ignore_region có reason — 9 polygon, mỗi cái đúng 1 reason (6 lens_border, 3 ego_body); không box nào nằm ≥50% trong ignore (kiểm bằng svm11.zones.ignored).
- [x] Tên task raw_fisheye và export CVAT 1.1 — export CVAT for images 1.1; tên task có raw_fisheye (xem xử lý cảnh báo ở trên).

## Fill ratio (K12)
- adasind_270517.jpg box 7 edge: 0.706
- adasind_270517.jpg box 1 center: 0.825
- adasind_310008.jpg box 4 edge: 0.530
- adasind_310008.jpg box 5 edge: 0.444
mean edge: 0.560 (n=3)
mean center: 0.825 (n=1)
