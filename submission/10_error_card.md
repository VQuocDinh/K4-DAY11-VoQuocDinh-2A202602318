# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | ATTRIBUTE | 1 |
| center | B4 | MISSING | 4 |
| center | B4 | SPURIOUS | 5 |
| center | C0 | BOX_GEOMETRY | 1 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B4 | BOX_GEOMETRY | 1 |
| edge | B4 | MISSING | 1 |
| edge | B4 | SPURIOUS | 1 |
| mid | B4 | BOX_GEOMETRY | 2 |
| mid | B4 | MISSING | 4 |
| mid | B4 | SPURIOUS | 14 |
| mid | B4 | WRONG_CLASS | 1 |

## Top defects
- SPURIOUS: 21 (ví dụ frame adasind_019560.jpg)
- MISSING: 10 (ví dụ frame adasind_019560.jpg)
- BOX_GEOMETRY: 4 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: Lỗi nổi bật nhất là **SPURIOUS ở zone mid (14 dòng)**, phần lớn là `M_only` của model chứ không phải của nhãn L. Mình xếp `E4_model_domain` vì cùng một cơ chế lặp lại ở cả 3 frame: YOLO26m gọi **6/6 xe ba bánh** là Car/Truck/Bus (258420 M4, M8, M11; 270517 M6, M7, M9; 310008 M6, M7 — có auto bị 2 box khác class), tách người lái khỏi xe máy (258420 M5, M10, trái R03) và box người ngồi trong auto (270517 M5, M10, M11). Đây là lệch taxonomy của model COCO, không phụ thuộc zone. Riêng nhãn L chỉ có 3 SPURIOUS + 2 MISSING, dồn ở `adasind_258420.jpg` mid: 1 lỗi thật của người gán (R7 Pedestrian bị sót, `E1`), còn lại nghi reference (L3 xe máy xa bị reference thiếu, L9/R5 reference lệch — `E0`) hoặc chưa đủ bằng chứng (L4 Truck, `E5`).
- Cách sửa và ai nhận việc (`owner`): `ai_team` — không dùng YOLO26m COCO làm pre-label cho class ThreeWheeler; fine-tune hoặc thêm bước ánh xạ/nhận diện riêng cho xe ba bánh, và hậu xử lý gộp người lái vào xe hai bánh, bỏ người trong xe (R03). `annotator` — rework R7 (đã làm, mid missing 2→1). `qa` — xác minh reference ở L3 (escalate) và R5.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `r3_diag/model_compare.md` (các dòng LR_noM của mọi auto), `r3_diag/local_quality.md` (ThreeWheeler L: precision=recall=1.000 trên 6 vật), các dòng `r3_diag` có `why=E4_model_domain` trong `findings.csv`, rule R03/R04; ảnh `screenshots/02_258420_L3_vs_R.png` cho cụm mid của 258420; `rework/delta.md`.
