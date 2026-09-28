# QA review · B4-dense

Mã khóa: 89B2-573F

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_258420.jpg | L4 | R04 | Truck (178,745)-(236,800), occluded: chỉ thấy phần nóc/khối tối phía sau xe máy và xe con trắng; không thấy thùng hàng/cabin nên class Truck chưa có căn cứ rõ trên ảnh. Cần đối chiếu hoặc xoá nếu không nhận ra. |
| adasind_270517.jpg | L6 | R01 | Bike (823,848)-(877,914) cạnh cột điện: vật xám tròn, không thấy rõ bánh/yên xe; có thể không phải phương tiện (đá/xô). Nếu không phải vật động thì box này thừa. |
| adasind_270517.jpg | L7 | R05 | Bike (1030,950)-(1080,1174) cắt ở mép phải: truncated=true đúng (bị khung ảnh cắt); cần soát cạnh dưới 1174 có vượt phần xe nhìn thấy không (R02) — polygon K12 dừng ~1180. |
| adasind_258420.jpg | L9 | R01 | Car (213,774)-(273,815) cao 41 px, sát ngưỡng H=40; lệch vài px khi kéo box có thể đổi phạm vi. Giữ nhưng ghi là ca biên. |
| adasind_270517.jpg | L4 | R05 | Car bạc (273,755)-(352,828) occluded=true vì người đi bộ L5 che một phần: đúng nghĩa occluded (vật khác che), không phải truncated. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.

Chế độ: solo — cold review chính bản đã khóa sau khi nghỉ, chưa mở teaching reference/model overlay/worked. Nhận xét chỉ ghi quan sát + luật, chưa chẩn đoán nguyên nhân.

## Trả lời ở P4 (sau khi mở reference)

| object_ref | Kết luận |
|---|---|
| 258420 L4 | R và M đều không có → `E5_unresolved`, giữ, cần frame lân cận/ảnh gốc để xác định class |
| 270517 L6 | Reference có R7 Bike ở cùng chỗ → nhận xét QA sai, giữ nhãn |
| 270517 L7 | Khớp R3 (IoU≈0.56), R3 rộng hơn về bên trái → giữ |
| 258420 L9 | R5 hẹp/lệch; L và M trùng → nghi reference (`E0`), giữ |
| 270517 L4 | Khớp R5; occluded không so với reference (R11) → giữ |

QA mù **không** phát hiện ca thực sự sai: người đi bộ R7 (258420) bị bỏ sót — sẽ rework.
