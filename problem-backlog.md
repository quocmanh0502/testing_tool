# Problem backlog

Những chỗ gặp trong lúc gán nhãn mà **guideline chưa trả lời được**, cộng các pain point về công cụ.

Ghi ngay khi gặp, kể cả lúc chưa biết xử lý thế nào. Một edge case không được ghi lại thì
mỗi người sẽ tự xử lý theo một kiểu — và đó là nguồn lớn nhất của nhãn không nhất quán.

> Các mục bên dưới là **ví dụ**, tên và link CVAT đều giả. Mẫu trống để copy nằm cuối file.

## Danh sách

| Mã | Tóm tắt | Loại | Mục guideline | Trạng thái | Kết quả |
|---|---|---|---|---|---|
| [P-001](#p-001) | Người ngồi sau xe máy: box riêng hay gộp với người lái | Guideline mơ hồ | §3.2 | ✅ Đã chốt | [QĐ-001](so-quyet-dinh.md#qđ-001) |
| [P-002](#p-002) | Xe bị che khuất hơn một nửa | Guideline chưa nói tới | §3.4 | ↗️ Hỏi BTC | — |
| [P-003](#p-003) | Phải vẽ lại box y hệt qua nhiều frame liên tiếp | Pain point công cụ | — | 🗣️ Đang bàn | — |

| [P-004](#p-004) | Phải bấm outside từng điểm trên khuôn mặt khi bị che hoặc ra khỏi khung, mất thời gian và dễ sót | Pain point công cụ | — | 🔴 Mở | — |
| [P-005](#p-005) | Chưa có hướng dẫn rõ ràng về cách gán nhãn tay trái khi guideline vẫn yêu cầu label tay trái | Guideline chưa nói tới | — | 🔴 Mở | — |
| [P-006](#p-006) | Lông mày không thể nhìn thấy khi gán nhãn khuôn mặt | Guideline chưa nói tới | — | 🔴 Mở | — |
**Loại**

| Loại | Nghĩa là |
|---|---|
| Guideline chưa nói tới | Tình huống không có trong guideline |
| Guideline mơ hồ | Đọc guideline ra được hai cách hiểu trở lên |
| Guideline mâu thuẫn | Hai mục trong guideline nói ngược nhau |
| Pain point công cụ | Guideline rõ, nhưng làm trên CVAT chậm hoặc dễ sai |

**Trạng thái:** 🔴 Mở · 🗣️ Đang bàn · ↗️ Hỏi BTC · ✅ Đã chốt (trỏ sang QĐ) · 🛠️ Làm tool (trỏ sang `source-tool/`) · ⚪ Bỏ (ghi lý do)

---

## P-001

**Người ngồi sau xe máy: box riêng hay gộp chung với người lái**

- **Loại:** Guideline mơ hồ
- **Mục guideline:** §3.2 — "mỗi người một bounding box"
- **Người phát hiện:** @thanh-vien-b · dd/mm/yyyy
- **Link CVAT:**
  - https://cvat.example.com/tasks/12/jobs/101?frame=37 — hai người, gần như chồng khít
  - https://cvat.example.com/tasks/12/jobs/101?frame=112 — người ngồi sau chỉ lộ đầu
- **Mô tả:** §3.2 nói mỗi người một box, nhưng hình minh hoạ trong guideline lại vẽ một box
  cho cả xe máy lẫn người trên xe.
- **Các cách hiểu:**
  1. Theo câu chữ: người ngồi sau có box `nguoi` riêng.
  2. Theo hình minh hoạ: không vẽ box `nguoi` cho ai đang ngồi trên xe.
- **Xử lý tạm trong lúc chờ:** vẽ box riêng và gắn tag `can_xem_lai` để dễ lọc ra sửa.
- **Kết quả:** ✅ [QĐ-001](so-quyet-dinh.md#qđ-001)

## P-002

**Xe bị che khuất hơn một nửa**

- **Loại:** Guideline chưa nói tới
- **Mục guideline:** §3.4 — chỉ nói về vật thể bị cắt ở mép ảnh, không nói về bị che
- **Người phát hiện:** @thanh-vien-c · dd/mm/yyyy
- **Link CVAT:**
  - https://cvat.example.com/tasks/12/jobs/103?frame=8 — ô tô sau xe buýt, lộ khoảng 30%
  - https://cvat.example.com/tasks/12/jobs/103?frame=64 — xe máy sau cột điện, lộ khoảng 50%
- **Mô tả:** Không rõ có gán nhãn vật thể bị che không, và nếu có thì box ôm phần nhìn thấy
  hay ôm cả phần ước lượng bị che.
- **Các cách hiểu:**
  1. Bỏ qua khi lộ dưới 50%.
  2. Luôn gán, box chỉ ôm phần nhìn thấy.
  3. Luôn gán, box ôm cả phần ước lượng.
- **Xử lý tạm trong lúc chờ:** dừng job 103, chuyển sang job khác ít ca che khuất.
- **Kết quả:** ↗️ Đã hỏi BTC ngày dd/mm/yyyy, chờ trả lời.

## P-003

**Phải vẽ lại box y hệt qua nhiều frame liên tiếp**

- **Loại:** Pain point công cụ
- **Mục guideline:** —
- **Người phát hiện:** @thanh-vien-d · dd/mm/yyyy
- **Link CVAT:** https://cvat.example.com/tasks/12/jobs/105?frame=200 — frame 200–260, xe đỗ không di chuyển
- **Mô tả:** Ảnh chụp liên tiếp từ camera cố định. Xe đỗ bên đường xuất hiện y nguyên ở hàng chục
  frame, annotator phải vẽ lại ở từng frame. Ước tính chiếm ~40% thời gian job 105.
- **Hướng đang cân nhắc:**
  1. Dùng chế độ *Track* sẵn có của CVAT — cần thử xem có hợp với dữ liệu dạng ảnh rời không.
  2. Viết script đọc file export của CVAT, nhân box sang các frame kế tiếp, rồi import lại.
- **Kết quả:** 🗣️ Đang bàn. Nếu chọn hướng 2 thì đổi trạng thái sang 🛠️ và làm trong
  [`source-tool/`](source-tool/).

## P-004

**Phải bấm outside từng điểm trên khuôn mặt khi bị che hoặc ra khỏi khung, mất thời gian và dễ sót**

- **Loại:** Pain point công cụ
- **Mục guideline:** —
- **Người phát hiện:** @hoangquocmanh2004 · 19/11/2026
- **Link CVAT:** http://localhost:8080/tasks/19/jobs/14?frame=0 — skeleton face 13 điểm
- **Mô tả:** Khi khuôn mặt trong task faces bị che hoặc ra khỏi khung hình, người dùng phải đánh dấu từng điểm trong 13 điểm của skeleton face là outside một cách thủ công. Việc này rất chậm và dễ bỏ sót điểm, gây khó khăn trong quá trình gán nhãn.
- **Hướng đang cân nhắc:**
  1. Cho phép đánh dấu toàn bộ các điểm trên khuôn mặt là outside cùng lúc khi khuôn mặt bị che hoặc ra khỏi khung
  2. Tự động nhận diện và đánh dấu các điểm không nhìn thấy là outside
  3. Giữ nguyên cách làm thủ công hiện tại nhưng thêm phím tắt hoặc công cụ hỗ trợ để đánh dấu nhanh hơn
- **Kết quả:** 🔴 Mở

## P-005

**Chưa có hướng dẫn rõ ràng về cách gán nhãn tay trái khi guideline vẫn yêu cầu label tay trái**

- **Loại:** Guideline chưa nói tới
- **Mục guideline:** —
- **Người phát hiện:** @hoangquocmanh2004 · 19/11/2026
- **Link CVAT:** http://localhost:8080/tasks/26/jobs/22?frame=0
- **Mô tả:** Người dùng thắc mắc về cách gán nhãn cho tay trái trong trường hợp guideline vẫn bắt label tay trái nhưng không có hướng dẫn cụ thể. Do guideline chưa đề cập nên gây khó khăn khi thực hiện nhãn, không biết có cần gán nhãn tay trái riêng hay theo cách nào.
- **Các cách hiểu:**
  1. Gán nhãn tay trái theo cách thông thường như tay phải
  2. Bỏ qua nhãn tay trái nếu không rõ cách làm
  3. Chờ hướng dẫn bổ sung từ guideline
- **Xử lý tạm trong lúc chờ:** Gán nhãn tay trái theo cách tương tự tay phải hoặc theo thỏa thuận nhóm
- **Kết quả:** 🔴 Mở

## P-006

**Lông mày không thể nhìn thấy khi gán nhãn khuôn mặt**

- **Loại:** Guideline chưa nói tới
- **Mục guideline:** —
- **Người phát hiện:** @hoangquocmanh2004 · 19/11/2026
- **Link CVAT:** http://localhost:8080/tasks/443/jobs/1823?frame=3
- **Mô tả:** Khi gán nhãn khuôn mặt, có trường hợp lông mày không thể nhìn thấy do góc quay hoặc che khuất. Hiện tại guideline không có hướng dẫn cụ thể về cách xử lý tình huống này, gây khó khăn cho người gán nhãn trong việc quyết định có nên bỏ qua hay đánh dấu như thế nào.
- **Các cách hiểu:**
  1. Bỏ qua phần lông mày khi không nhìn thấy
  2. Dự đoán vị trí lông mày dựa trên các đặc điểm khuôn mặt còn lại
  3. Đánh dấu đặc biệt cho các trường hợp lông mày bị che khuất
- **Xử lý tạm trong lúc chờ:** Bỏ qua phần lông mày khi không thể nhìn thấy để tiếp tục gán nhãn các phần khác
- **Kết quả:** 🔴 Mở

---

## Mẫu để copy

```markdown
## P-NNN

**Tóm tắt một dòng**

- **Loại:** Guideline chưa nói tới | Guideline mơ hồ | Guideline mâu thuẫn | Pain point công cụ
- **Mục guideline:** §
- **Người phát hiện:** @ · dd/mm/yyyy
- **Link CVAT:** (bỏ trống nếu không có)
  - https://…/tasks/<id>/jobs/<id>?frame=<n> — frame này có gì
- **Mô tả:**
- **Các cách hiểu:** (với pain point công cụ thì ghi **Hướng đang cân nhắc:**)
  1.
  2.
- **Xử lý tạm trong lúc chờ:**
- **Kết quả:** 🔴 Mở
```

Nhớ thêm một dòng vào bảng **Danh sách** ở đầu file.
