# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 6 | 0 | 0 | 4 | 5 | — |
| mid | 7 | 2 | 3 | 2 | 9 | SPURIOUS (2) |
| edge | 7 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: **mid** gãy nhiều nhất cho cả hai. L: 2 missing + 3 spurious ở mid, còn center 0/0 và edge 0/0 (n_ref 6 và 7). M: mid có 9 box thừa và 2 thiếu; center 4 thiếu + 5 thừa. Toàn bộ lỗi của L nằm ở **một frame** `adasind_258420.jpg` (local_quality: frame này TP=6, FP=3, FN=2; hai frame còn lại precision=recall=1.000).
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: Lỗi L ở mid **không** do méo fisheye mà do **mật độ**: ở 258420 vùng mid là cụm xe máy, người đi bộ và xe con xa, chồng lấn nhau và nhiều vật sát ngưỡng H=40 (L3 H=47, L9 H=41, R7 H=51) — dễ bỏ sót (R7) hoặc lệch với reference (L3, L9/R5 nghi reference sai). Lỗi M có cơ chế khác: gọi sai class ThreeWheeler thành Car/Truck/Bus ở 6/6 auto (sinh cả LR_noM lẫn M_only), tách người lái khỏi xe (M5, M10 — ngược R03) và box người ngồi trong auto (270517 M5/M10/M11); đó là lệch taxonomy/miền của model COCO, không phải do vị trí zone. Giới hạn: chỉ 3 frame, 20 vật reference, lỗi L dồn vào 1 frame nên không suy được tỷ lệ lỗi theo zone; ngưỡng IoU cũng làm đổi kết quả (iou_sweep: L mid matched 6 ở 0.3 → 3 ở 0.7; L edge 7 → 5), tức box vật nhỏ ở mid/edge nhạy với ngưỡng; reference là teaching reference, có thể sai (L3, R5).
