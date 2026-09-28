# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 6 | 6 | 0 | 0 | 0 | 0 |
| mid | 5 | 6 | 2 | 1 | 3 | 3 |
| edge | 7 | 7 | 0 | 0 | 0 | 0 |

## Findings action=rework
- adasind_258420.jpg R7 MISSING: đã sửa
- adasind_258420.jpg R7+M12 MISSING: đã sửa

## Nhận xét

Chỉ sửa 1 ca có căn cứ (`action=rework`, P1): thêm Pedestrian R7 ở `adasind_258420.jpg`. Zone mid: matched 5 → 6, missing 2 → 1; spurious giữ 3 vì các ca còn lại là quyết định giữ có lý do (L3 escalate E0, L4 E5, L9 lệch reference E0 — D02–D04), không sửa để khớp số. center/edge không đổi (6/6 và 7/7). Missing còn lại là R5 — cùng vật với L9, nghi reference.
