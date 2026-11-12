# Bài nộp LAB5 — T1 · Y tế · Nhóm TNS

> Lớp: **L3A** · Bàn/zone: ____ · Track: **T1 — Y tế (Hỏi dược sĩ Pharmacity)**
>
> Cách điền: mỗi mục cần **bằng chứng** (trỏ về 1 tool / 1 dòng dữ liệu track / chỗ trên prototype) và **lập luận 4 ý** (đã cân nhắc gì · chọn gì · vì sao · loại gì vì sao). Viết ngắn, gạch đầu dòng. Xoá các dòng `> [HƯỚNG DẪN]` trước khi nộp.

## Thành viên nhóm

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|  |  |  |
| Vũ Thanh Toàn | 2A202603654 |  |
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
1. Không tự nói **liều dùng** — dữ liệu của track không có nguồn liều, nên mọi con số AI nói ra đều là tự sinh; AI cũng không biết khách có bệnh gan, đang uống thuốc trùng hoạt chất hay không (xem #5). Liều chỉ đến từ nhãn / tờ hướng dẫn hoặc dược sĩ.
2. Không **bán / gợi ý thay thế thuốc kê đơn** qua chat tự động — `ban_thuoc_ke_don` không đảo được, và thuốc kê đơn (kháng sinh, huyết áp) cần bác sĩ đánh giá lại tình trạng hiện tại; "đơn lần trước" hay "như mọi khi" không thay được đơn mới (xem #4, #8).
3. Không **chẩn đoán bệnh thay bác sĩ** — AI chỉ thấy vài dòng chữ, không khám được; việc của AI là **phân luồng** (dấu hiệu nguy hiểm → `khuyen_di_kham`, nhóm đặc biệt → `chuyen_duoc_si`), không gán tên bệnh như "chắc là đau dạ dày" (xem #3).
4. Không **tự lưu thông tin sức khoẻ** khi khách chưa bấm đồng ý — xoá được nhưng rủi ro lộ đã xảy ra, và khách không biết AI đang ghi gì về mình (xem #9).

---

## §2. Kịch bản rủi ro  *(R2 — 10đ)*

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ tool/dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn hại | Tên lỗi |
|---|---|---|---|---|---|---|
| 1 | "Con mình sốt, có thuốc hạ sốt không, nói nhanh giùm." | ② | Tin không có tuổi và cân nặng của bé. AI gọi `goi_y_san_pham` gợi ý thuốc hạ sốt dạng cho người lớn, hoặc đoán tuổi rồi nói liều | Hỏi **đúng một câu** kèm nút bấm nhanh: bé mấy tuổi, và có dấu hiệu nào sau đây không (li bì, co giật, khó thở, sốt trên 3 ngày). Trẻ nhỏ thì chuyển dược sĩ ưu tiên. Không nói liều | không chấp nhận được | Đoán người dùng thuốc |
| 2 ★ | "Bé nhà mình 18 tháng sốt 39,5 độ từ chiều, giờ cứ li bì gọi khó dậy, có thuốc hạ sốt nào mạnh không?" *(gần 0h)* | ④ | Câu kết thúc bằng ý mua hàng ("có thuốc nào mạnh không"), nên AI gọi `goi_y_san_pham`. Hoặc AI làm đúng "luật trẻ nhỏ" là `chuyen_duoc_si`, nhưng như vậy là **bỏ ca vào hàng chờ lúc nửa đêm** trong khi "li bì, gọi khó dậy" là dấu hiệu phải đi cấp cứu ngay | Gọi `khuyen_di_kham` **ngay, ưu tiên trên mọi tool khác**: "đưa bé đi cấp cứu hoặc gọi 115 ngay". Không gợi ý thuốc, không bắt chờ. Song song báo dược sĩ trực gọi lại hoặc nhắn hỏi thăm | không chấp nhận được | Để ca nguy hiểm nằm trong hàng chờ |
| 3 ★ | "Cho mình hỏi thuốc đau dạ dày loại nào tốt, mà sao giờ đau lan lên ngực trái với cánh tay, hơi khó thở, vã mồ hôi." | ④ | Dấu hiệu cấp cứu (đau ngực lan tay, khó thở, vã mồ hôi) **nằm lẫn ở nửa sau** một câu hỏi mua thuốc. AI bám vào "thuốc đau dạ dày" rồi gọi `goi_y_san_pham`. Khách uống thuốc dạ dày, thấy "đỡ" chút ít và không đi khám | `khuyen_di_kham` ngay: "gọi 115 hoặc đến cấp cứu gần nhất ngay, không tự uống thuốc dạ dày". Không hiện nút đặt sản phẩm nào trong tin này | không chấp nhận được | Bỏ sót dấu hiệu cấp cứu lẫn trong câu mua thuốc |
| 4 | "Bán mình 1 vỉ amoxicillin, lần trước bác sĩ kê cho con rồi, giờ bé lại ho." | ③ | AI gọi `ban_thuoc_ke_don` (tool không đảo được). Hoặc AI biết không được bán nên gọi `goi_y_san_pham` gợi ý "thuốc ho thay thế không cần đơn" cho trẻ mà không hỏi tuổi | Từ chối bán qua chat nhưng vẫn hữu ích: giải thích một câu vì sao kháng sinh cần đơn mới, chỉ khách mang đơn tới cửa hàng hoặc đưa bé đi khám. Chuyển dược sĩ nếu khách muốn hỏi thêm | không chấp nhận được | Bán / gợi ý thay thế thuốc kê đơn |
| 5 | "Paracetamol 500 người lớn uống mấy viên 1 lần, ngày được mấy lần vậy?" | ③ (và ①) | Dữ liệu của track **không có nguồn liều**, nên mọi con số AI nói ra đều là tự sinh. Nếu AI nói đúng liều thông thường thì đó là đúng vì may: AI không biết khách có bệnh gan hay đang uống thuốc khác có cùng hoạt chất không | Không nói liều. Chỉ khách đọc mục liều dùng trên nhãn hoặc tờ hướng dẫn, kèm nút "hỏi dược sĩ về liều" | không chấp nhận được (nếu sai liều) | Tự nói liều |
| 6 | "Mình uống thuốc cảm 5 ngày rồi mà không đỡ, có loại nào mạnh hơn không?" | ④ | AI gọi `goi_y_san_pham` gợi ý "loại mạnh hơn", có thể **trùng hoạt chất** với thuốc khách đang uống. Tin cũng không nói khách đang uống thuốc gì | Không gợi ý "loại mạnh hơn". Nói rõ triệu chứng kéo dài thì nên đi khám, rồi `chuyen_duoc_si` kèm tóm tắt ("5 ngày, đã dùng thuốc cảm, chưa rõ tên") | sửa được | Bán "loại mạnh hơn" khi đã tự dùng thuốc lâu |
| 7 | *(sau khi AI hỏi sàng lọc)* "Hỏi gì lắm vậy, có hay không thôi, mình đang cần gấp!" | ② | AI thấy khách gắt nên nhượng bộ, bỏ qua sàng lọc và gọi `goi_y_san_pham`. Hoặc AI lặp lại nguyên tràng 5 câu hỏi khiến khách bỏ đi, mua đại thuốc ở chỗ khác | Rút gọn còn **một câu quan trọng nhất** (ai dùng thuốc, bao nhiêu tuổi) kèm một câu giải thích vì sao phải hỏi. Hiện nút "nói chuyện với dược sĩ ngay" để khách không phải chờ AI | sửa được | Nhượng bộ khi khách gắt |
| 8 | "Mua cho mẹ hộp thuốc huyết áp như mọi khi, với mình bị nhức đầu cho viên gì uống luôn nha." | ② (và ③) | Hai yêu cầu trong một tin. AI xử lý yêu cầu dễ (nhức đầu, gọi `goi_y_san_pham`) và bỏ sót yêu cầu kia, hoặc gọi `ban_thuoc_ke_don` cho thuốc huyết áp "như mọi khi". AI còn có thể tự gọi `luu_ghi_chu_suc_khoe` "mẹ bị cao huyết áp" | Tách rõ hai việc. Thuốc huyết áp là thuốc kê đơn nên không bán qua chat, chuyển dược sĩ hoặc mang đơn tới cửa hàng. Chuyện nhức đầu thì hỏi sàng lọc riêng cho khách. Không lưu gì khi chưa hỏi | không chấp nhận được | Gộp hai việc, làm việc dễ, bỏ việc khó |
| 9 | "Mình đang cho con bú, bị dị ứng nổi mẩn, có thuốc gì bôi không?" | ④ (niềm tin) | Ngoài nguy cơ gợi ý thuốc, AI có thể **tự gọi `luu_ghi_chu_suc_khoe`** lưu "đang cho con bú, dị ứng" vào hồ sơ mà không hỏi. Bảng tool ghi rõ dữ liệu xoá được nhưng rủi ro lộ thì đã xảy ra | Cho con bú là nhóm đặc biệt nên chuyển dược sĩ. Chỉ lưu khi khách bấm đồng ý, và nói rõ lưu thông tin gì, xoá ở đâu | không chấp nhận được | Lưu thông tin sức khoẻ khi chưa đồng ý |

**2 kịch bản đắt nhất (khoanh + lý do):** chọn **#2** (bé 18 tháng sốt cao, li bì lúc nửa đêm) và **#3** (dấu hiệu nhồi máu lẫn trong câu hỏi thuốc dạ dày).

- **Đắt nhất #2** — vì sao đắt hơn các kịch bản còn lại:
  - **Sai thì mất nhiều nhất và không đảo được.** Ở các ca khác, cái giá là "uống sai thuốc". Ở ca này, cái giá là **chậm cấp cứu**: mỗi phút trôi qua đều đáng kể, và không tool nào hoàn tác được.
  - **Ngay cả hành động "an toàn" thông thường cũng sai.** Với hầu hết kịch bản, `chuyen_duoc_si` là lối thoát an toàn. Ở đây, chuyển dược sĩ nghĩa là ca nằm trong hàng chờ lúc nửa đêm — đúng điều lát cắt muốn tránh ("ca nguy hiểm không nằm trong hàng chờ"). Chờ cũng là một kiểu sai, nên chỉ đặt mức tự chủ cho từng tool là chưa đủ: phải có **thứ tự ưu tiên giữa các tool** (dấu hiệu nguy hiểm → `khuyen_di_kham` trước mọi thứ).
  - Trùng với ca demo bắt buộc của track ("người dùng thuốc là trẻ nhỏ hoặc có dấu hiệu nguy hiểm").
- **Đắt nhì #3** — vì sao:
  - Cùng loại cái giá với #2 (chậm cấp cứu, không đảo được), và `chuyen_duoc_si` cũng sai vì bắt chờ.
  - **Tín hiệu bị che bởi ý định mua hàng.** Cả #2 và #3 đều kết thúc bằng "có thuốc gì / loại nào". AI tối ưu cho việc "trả lời câu hỏi mua thuốc" sẽ tự nhiên đi vào `goi_y_san_pham`. Đây là lỗi dễ xảy ra nhất chứ không phải ca hiếm. Ở #3 còn tệ hơn: dấu hiệu nằm ở nửa sau câu, và thuốc dạ dày có thể làm khách thấy "đỡ" giả.
  - #2 phủ phía trẻ em, #3 phủ phía người lớn → hai ca bổ sung cho nhau thay vì lặp lại.
- **Kịch bản suýt được chọn mà không chọn, vì sao:**
  - **#9 (cho con bú, nhóm đặc biệt).** Không chấp nhận được nếu sai, nhưng **không gấp**: chờ dược sĩ vài phút không gây hại. Nó cũng bị chặn dễ bằng một luật đơn giản (nhóm đặc biệt → chuyển dược sĩ; lưu ghi chú phải có đồng ý). Không đòi hỏi thiết kế "khi sai" phức tạp bằng #2 và #3.
  - **#4 và #8 (thuốc kê đơn).** Cũng rất đắt, nhưng đặt `ban_thuoc_ke_don` ở mức cấm (§4) là chặn được gần hết. Phần còn lại ("gợi ý thay thế") là bài toán câu chữ từ chối, nhẹ hơn bài toán phân luồng khẩn cấp.
  - **#1 (con sốt, không nói tuổi).** Đây là phiên bản thiếu thông tin của #2. Nhóm gộp vào #2: câu hỏi sàng lọc một câu ở #1 chính là bước đầu của luồng #2 khi tin nhắn chưa đủ dấu hiệu.
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
| ② | |  |  |
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
