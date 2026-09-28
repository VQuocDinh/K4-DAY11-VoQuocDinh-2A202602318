# Escalation ticket

## Ticket 1

- **Frame:** `adasind_258420.jpg` (slice B4-dense), object `L3` — xe máy có người lái áo tối, box (171,800)-(193,847), cao 47 px, zone mid, ngay sau xe con trắng. Reference không có box nào tương ứng (finding `r3_diag`/`r1_craft` L3, `cell=L_only`, `why=E0_reference_defect`).
- **Ảnh chụp:** `submission/screenshots/02_258420_L3_vs_R.png` (vàng = L, tím = R; L3 ở giữa ảnh, không có khung tím tương ứng).
- **Expected impact:** vật cao ≥ H=40 trong vùng hợp lệ nên theo R01 phải có box; nếu reference không sửa, mọi báo cáo so với reference sẽ tính L3 là SPURIOUS (FP) và hạ precision Bike của slice (local_quality Bike precision 0.800 thay vì 1.000), đồng thời phạt người gán đúng luật. Cụm mid dày của 258420 cũng là nơi model và reference cùng thiếu (M cũng không có L3) nên nếu dùng reference làm gold, lỗi thiếu xe máy nhỏ ở mid sẽ bị che giấu.
- **Owner:** `qa` (người duy trì teaching reference / Lab Coach)
- **Recommendation:** thêm box `Bike` cho xe máy + người lái tại ≈(171,800)-(193,847) vào teaching reference B4-dense (bump version reference), và soát lại cùng frame R5 Car (219,782)-(257,824) — box hẹp/lệch so với xe con trắng (L9 và model M6 cùng ranh giới ≈(213,774)-(273,815)). Sau khi sửa, chạy lại `python3 lab11.py compare r1_craft` và `local-quality` để cập nhật số.
