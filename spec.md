# Bài nộp LAB5 — T1 · Y tế · Nhóm TNS

> Lớp: **L3A** · Bàn/zone: ____ · Track: **T1 — Y tế (Hỏi dược sĩ Pharmacity)**
>
> Cách điền: mỗi mục cần **bằng chứng** (trỏ về 1 tool / 1 dòng dữ liệu track / chỗ trên prototype) và **lập luận 4 ý** (đã cân nhắc gì · chọn gì · vì sao · loại gì vì sao). Viết ngắn, gạch đầu dòng. Xoá các dòng `> [HƯỚNG DẪN]` trước khi nộp.

## Thành viên nhóm

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|  |  |  |
|  |  |  |
|  |  |  |

> [HƯỚNG DẪN] Ghi cụ thể việc mỗi người làm — khi demo ai cũng có thể bị hỏi về phần có tên mình. Mã học viên phải đúng (căn cứ đối chiếu điểm).

---

## §1. Lát cắt & phạm vi  *(R1 — 5đ)*

**Lát cắt (chép từ track, không đổi):**
Trong kênh chat Hỏi dược sĩ của Pharmacity, khi **khách** nhắn hỏi "tôi (hoặc người nhà) bị X, có thuốc gì không", **AI** tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ / khuyên đi khám ngay — để **người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ**.

| Người dùng | Việc | Quyết định của AI | Kết quả |
|---|---|---|---|
| khách | nhắn hỏi "tôi/người nhà bị X, có thuốc gì không" | gợi ý sản phẩm / hỏi sàng lọc / chuyển dược sĩ / khuyên đi khám | không nhận sai thuốc, ca nguy hiểm không bị kẹt hàng chờ |

**3 việc tính năng KHÔNG làm (mỗi việc 1 câu vì sao):**
1. Không tự nói **liều dùng** — … *(vì sao)*
2. Không **bán / gợi ý thay thế thuốc kê đơn** qua chat tự động — …
3. Không **chẩn đoán bệnh thay bác sĩ** — …

> [HƯỚNG DẪN] Prototype không được vi phạm 3 điều này. Có thể thêm việc thứ 4 nếu muốn.

---

## §2. Kịch bản rủi ro  *(R2 — 10đ)*

> [HƯỚNG DẪN] ≥8 kịch bản, phủ đủ 4 lớp (mỗi lớp ≥1). Cột "AI có thể sai thế nào" **phải trỏ về 1 tool hoặc 1 dòng dữ liệu** (hoặc chỗ dữ liệu KHÔNG có). Nhãn: nhẹ / sửa được / không chấp nhận được. Tool có: `goi_y_san_pham`, `chuyen_duoc_si`, `khuyen_di_kham`, `luu_ghi_chu_suc_khoe`, `ban_thuoc_ke_don`.

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ tool/dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn hại |
|---|---|---|---|---|---|
| 1 | "Con mình sốt, có thuốc gì hạ sốt không" | ② | AI gọi `goi_y_san_pham` ngay vì tin không nói tuổi/cân nặng trẻ | hỏi sàng lọc tuổi/cân nặng trước, không gợi ý ngay | không chấp nhận được |
| 2 |  | ① |  |  |  |
| 3 |  | ③ |  |  |  |
| 4 |  | ④ |  |  |  |
| 5 |  |  |  |  |  |
| 6 |  |  |  |  |  |
| 7 |  |  |  |  |  |
| 8 |  |  |  |  |  |

**2 kịch bản đắt nhất (khoanh + lý do):**
- Đắt nhất #__: vì sao đắt hơn các kịch bản còn lại — …
- Đắt nhì #__: vì sao — …
- Kịch bản nào **suýt được chọn** mà không chọn, vì sao — …

> [HƯỚNG DẪN] Hard tests gợi ý: người dùng thuốc ≠ người nhắn (trẻ nhỏ/người già/bà bầu) · dấu hiệu cấp cứu lẫn trong câu mua thuốc · đòi thuốc kê đơn không đơn · khách gắt khi bị hỏi lại · đã tự uống nhiều ngày không đỡ · hai yêu cầu trong một tin.

---

## §3. Khi AI sai  *(R3 — 25đ · phần chính)*

> [HƯỚNG DẪN] Chỉ làm cho **2 kịch bản đắt nhất** ở §2.

### Kịch bản đắt nhất 1 — #__

- **AI nói (nguyên văn):** "…"
- **Người dùng thấy gì:** độ chắc / bằng chứng / nút / nhãn — …
- **Làm gì tiếp được:** sửa / bỏ qua / hỏi người / hoàn tác — …
- **Ai được báo, sau bao lâu, thấy gì:** …
- **Quyết định:** hỏi lại / trả lời kèm giới hạn / dừng & chuyển người — chọn …, loại … vì …
- **Loại lỗi:** hệ thống / người dùng / ngữ cảnh — và cách nói, cách sửa theo loại đó …
- **Nguyên tắc HAX/PAIR (2–3), mỗi cái trỏ chỗ cụ thể:**
  - G__ → …
  - G__ → …
- **Lỗi sửa câu chữ không cứu được (nếu có) + thiết kế cứu nó:** …

### Kịch bản đắt nhất 2 — #__

*(lặp cùng khung trên)*

---

## §4. Mức tự chủ  *(R4 — 25đ · phần chính)*

> [HƯỚNG DẪN] Mỗi tool một mức + lý do theo cost-of-error (sai thì ai chịu gì · đảo được không · chờ người mất gì). KHÔNG viết "vì tiện", "vì AI giỏi".

| Tool | Đảo lại được? | Mức của nhóm | Lý do theo cost-of-error | Phương án đã loại & vì sao |
|---|---|---|---|---|
| `goi_y_san_pham` | Không (khi đã dùng) |  |  |  |
| `chuyen_duoc_si` | Được, tốn vài phút |  |  |  |
| `khuyen_di_kham` | Khách bỏ qua được |  |  |  |
| `luu_ghi_chu_suc_khoe` | Xoá được, nhưng đã có rủi ro lộ |  |  |  |
| `ban_thuoc_ke_don` | Không |  |  |  |

**Nếu "cần người duyệt":** ai duyệt · người duyệt thấy gì trước khi bấm · quá hạn không ai duyệt thì sao — …

**Tool khó xếp nhất:** tool `____` — mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản §2), nhóm chốt thế nào — …

**Thử mức trên 3 kịch bản** (2 đắt nhất + 1 bình thường):
| Kịch bản | Cái sai có bị chặn? | Có việc nào AI vẫn làm khi lẽ ra phải hỏi? | Người dùng bình thường có phải chờ vô ích? |
|---|---|---|---|
| đắt nhất 1 |  |  |  |
| đắt nhất 2 |  |  |  |
| bình thường |  |  |  |

**Luật thêm / tool tách bước (nếu có):** … (ví dụ tách "soạn gợi ý" khỏi "gửi gợi ý")
**Một mức tự chủ đã đổi lời hứa nào với người dùng:** … ("AI tự tư vấn" ≠ "AI soạn, dược sĩ duyệt")

---

## §5. Prototype  *(R5 — 25đ · phần chính)*

- **Link hoặc file:** `prototype/` (xem README trong thư mục)
- **Công cụ:** … · **Phần nào mock:** … · **Có lời gọi AI thật không:** …
- **Ảnh đánh số** (để trong `prototype/`):

| Số trên ảnh | Thể hiện gì | Nguyên tắc HAX/PAIR | Mức tự chủ |
|---|---|---|---|
| ① |  |  |  |
| ② |  |  |  |
| ③ |  |  |  |

- **Quyết định:** cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ cách nào khác, vì sao chọn cách này — …

---

## §6. Tốt là gì  *(R6 — 5đ · nhẹ, bỏ được nếu thiếu giờ)*

3 tiêu chí Pass/Fail chấm được:
1. …
2. …
3. …

Chấm thử thiết kế của nhóm trên 2 kịch bản đắt nhất + lỗi còn lại: …

---

## §7. Demo  *(R7 — 5đ)*

- **Ca lạ nhận ở demo:** … · prototype đã làm gì · nhóm có đổi ý gì sau ca đó không — …
- **Phân công demo (mỗi người 1 phần):** …
