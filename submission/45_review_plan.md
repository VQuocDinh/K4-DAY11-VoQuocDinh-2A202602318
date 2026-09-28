# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Cụm đông vật zone **mid**, `adasind_258420.jpg` (xe máy, người đi bộ, xe con xa sát ngưỡng H=40) | L: 2 MISSING + 3 SPURIOUS (toàn bộ lỗi L của slice); reference nghi sai 2 ca (L3, R5); 1 ca E5 (L4) | Vật nhỏ, chồng lấn, sát H=40 là nơi cả người, model và reference cùng lệch; một lỗi ở đây đổi precision/recall cả frame (0.667/0.750 so với 1.000 ở hai frame khác) | `r3_diag/local_quality_conflicts.csv`, `screenshots/02_258420_L3_vs_R.png`, `rework/delta.md`, dòng findings L3/L4/L9/R7 |
| **Xe ba bánh** mọi frame (6 auto ở 258420, 270517, 310008) | M: 6/6 sai class (Car/Truck/Bus) → 6 LR_noM + 8 M_only; L đúng 6/6 | Class riêng của miền dữ liệu (R04) mà model COCO không có; nếu dùng model pre-label/đánh giá mà không soát, lỗi class này sẽ lan sang cả tập | `r3_diag/model_compare.md`, dòng findings `E4_model_domain`, `local_quality.md` bảng theo nhãn |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame, 20 vật reference, một camera fisheye gắn trên xe hai bánh; lỗi L dồn vào 1 frame nên không suy được tỷ lệ lỗi theo zone hay theo class; zone center/mid/edge là vị trí trên ảnh, không phải khoảng cách tới xe; số đo phụ thuộc ngưỡng IoU (iou_sweep: L mid matched 6 ở 0.3 → 3 ở 0.7) và dựa trên teaching reference có thể sai.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: chia 50.000 frame theo `camera_id` × chuyến đi/cảnh (drive/scene id) × điều kiện (ngày/đêm, mưa, đông vật, bãi đỗ) rồi lấy mẫu phân tầng trong từng ô normal/hard; mỗi cảnh chỉ lấy tối đa 1–2 frame cách nhau đủ xa (ví dụ ≥ vài giây) để **không đếm nhiều frame liền nhau của cùng cảnh như nhiều ca độc lập**; lập bảng đếm theo camera × điều kiện × class có mặt (đặc biệt ThreeWheeler, xe hai bánh có người lái, vật sát ngưỡng H) để thấy ô trống. Kế hoạch này chọn mẫu có **chủ đích nghiêng về ca khó** nên chỉ giúp **tìm ca cần soi** và lỗi hệ thống; nó không phải mẫu ngẫu nhiên đại diện nên **chưa đo được tỷ lệ lỗi** của toàn bộ 50.000 frame — muốn ước lượng tỷ lệ cần một mẫu ngẫu nhiên riêng có trọng số theo tầng.
