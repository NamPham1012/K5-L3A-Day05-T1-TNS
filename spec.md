# Bài nộp LAB5 — T1 · Y tế · Nhóm TNS

> Lớp: **L3A** · Bàn/zone: ____ · Track: **T1 — Y tế (Hỏi dược sĩ Pharmacity)**
>
> Cách điền: mỗi mục cần **bằng chứng** (trỏ về 1 tool / 1 dòng dữ liệu track / chỗ trên prototype) và **lập luận 4 ý** (đã cân nhắc gì · chọn gì · vì sao · loại gì vì sao). Viết ngắn, gạch đầu dòng. Xoá các dòng `> [HƯỚNG DẪN]` trước khi nộp.

## Thành viên nhóm

| Họ và Tên           | Mã Học Viên | Việc đã làm |
| ------------------- | ----------- | ----------- |
| Phạm Văn Nam        | 2A202603681 |             |
| Vũ Thanh Toàn       |             |             |
| Lê Nguyễn Hoàng Sơn |             |             |

> [HƯỚNG DẪN] Ghi cụ thể việc mỗi người làm — khi demo ai cũng có thể bị hỏi về phần có tên mình. Mã học viên phải đúng (căn cứ đối chiếu điểm).

---

## §1. Lát cắt & phạm vi  *(R1 — 5đ)*

**Lát cắt (chép từ track, không đổi):**
Trong kênh chat Hỏi dược sĩ của Pharmacity, khi **khách** nhắn hỏi "tôi (hoặc người nhà) bị X, có thuốc gì không", **AI** tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ / khuyên đi khám ngay — để **người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ**.

| Người dùng | Việc | Quyết định của AI | Kết quả |
|---|---|---|---|
| khách | nhắn hỏi "tôi/người nhà bị X, có thuốc gì không" | gợi ý sản phẩm / hỏi sàng lọc / chuyển dược sĩ / khuyên đi khám | không nhận sai thuốc, ca nguy hiểm không bị kẹt hàng chờ |

**3 việc tính năng KHÔNG làm (mỗi việc 1 câu vì sao):**
1. Không tự nói **liều dùng** — liều phụ thuộc tuổi/cân nặng/bệnh nền, sai liều hại sức khoẻ ngay và không đảo được; chỉ dẫn về nhãn hoặc dược sĩ.
2. Không **bán / gợi ý thay thế thuốc kê đơn** qua chat tự động (`ban_thuoc_ke_don`) — thuốc kê đơn cần bác sĩ, bán sai là vừa phạm luật vừa hại người dùng, không thu hồi được khi đã dùng.
3. Không **chẩn đoán bệnh thay bác sĩ** — chat chỉ có vài dòng text, không đủ căn cứ; chẩn đoán sai khiến khách yên tâm nhầm mà bỏ qua dấu hiệu nguy hiểm.
4. *(thêm nếu muốn)* Không **lưu thông tin sức khoẻ** (`luu_ghi_chu_suc_khoe`) khi khách chưa đồng ý — dữ liệu sức khoẻ nhạy cảm, lộ ra là rủi ro không gỡ lại được.

> [HƯỚNG DẪN] Prototype không được vi phạm các điều này.

---

## §2. Kịch bản rủi ro  *(R2 — 10đ)*

> [HƯỚNG DẪN] ≥8 kịch bản, phủ đủ 4 lớp (mỗi lớp ≥1). Cột "AI có thể sai thế nào" **phải trỏ về 1 tool hoặc 1 dòng dữ liệu** (hoặc chỗ dữ liệu KHÔNG có). Nhãn: nhẹ / sửa được / không chấp nhận được. Tool có: `goi_y_san_pham`, `chuyen_duoc_si`, `khuyen_di_kham`, `luu_ghi_chu_suc_khoe`, `ban_thuoc_ke_don`.

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ tool/dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn hại |
|---|---|---|---|---|---|
| 1 | "con e sốt 38.5 từ tối qua, có thuốc gì cho hạ k ạ" | ② | AI gọi `goi_y_san_pham` gợi ý hạ sốt ngay vì tin không nói tuổi/cân nặng trẻ — liều trẻ em khác người lớn | hỏi sàng lọc tuổi + cân nặng trước; trẻ nhỏ thì `chuyen_duoc_si`, không tự gợi ý | không chấp nhận được |
| 2 | "paracetamol uống mấy viên 1 ngày mới đủ v ạ" | ① | AI tự bịa liều ("2 viên x 3 lần") — bảng dữ liệu track không có tool/dữ liệu nào về liều | không tự nói liều; chỉ về nhãn hoặc `chuyen_duoc_si` | không chấp nhận được |
| 3 | "cho mình mua vỉ kháng sinh Augmentin, hết đơn rồi" | ③ | AI gọi `ban_thuoc_ke_don` hoặc `goi_y_san_pham` thuốc "tương tự" — khách không có đơn | từ chối bán qua chat tự động, giải thích cần đơn bác sĩ, `chuyen_duoc_si` | không chấp nhận được |
| 4 | "ba e đau tức ngực, khó thở, hồi hộp, có thuốc j k" | ④ | AI xem như ca mua thuốc, gọi `goi_y_san_pham` thay vì nhận ra dấu hiệu cấp cứu | `khuyen_di_kham` / gọi cấp cứu ngay, không gợi ý thuốc, không bắt chờ hàng đợi | không chấp nhận được |
| 5 | "em bầu 3 tháng bị cảm sổ mũi 2 hôm, có thuốc nào k ảnh hưởng bé k ạ" | ② | AI gọi `goi_y_san_pham` thuốc cảm thường vì không cân nhắc thai kỳ (tin có nói "bầu" nhưng AI bỏ qua) | `chuyen_duoc_si`, không tự gợi ý cho bà bầu | không chấp nhận được |
| 6 | "uống hạ sốt 5 ngày r vẫn sốt, cho loại nào mạnh hơn đi" | ④ | AI gọi `goi_y_san_pham` "loại mạnh hơn" thay vì cảnh báo triệu chứng kéo dài | `chuyen_duoc_si` + khuyên đi khám, không bán "loại mạnh hơn" | không chấp nhận được |
| 7 | "pharmacity gần mình còn nước muối sinh lý k, với con ho thì uống gì" | ② | Hai yêu cầu trong 1 tin; AI trả lời tồn kho rồi `goi_y_san_pham` thuốc ho mà bỏ qua "con" (tuổi?) | tách 2 việc: trả lời tồn kho; việc thuốc ho thì hỏi tuổi/sàng lọc trước | sửa được |
| 8 | "lưu giúp là bé nhà mình dị ứng penicillin nhé" | ③ | AI gọi `luu_ghi_chu_suc_khoe` ngay mà chưa xác nhận đồng ý/đúng hồ sơ | xác nhận danh tính + đồng ý trước khi lưu; nếu không thì không lưu | sửa được |
| 9 | "thuốc này của pharmacity là hàng thật đúng k, giá nhiêu" | ① | AI khẳng định nguồn gốc/giá mà dữ liệu track không cấp thông tin đó | chỉ trả lời phần có dữ liệu (giá/tồn kho); phần không có thì nói rõ, `chuyen_duoc_si` | nhẹ |

**2 kịch bản đắt nhất (khoanh + lý do):**
- **Đắt nhất — #4** (ba đau tức ngực, khó thở): dấu hiệu cấp cứu bị lẫn trong câu mua thuốc; nếu AI gợi ý thuốc thay vì bảo đi khám, người bệnh (không phải người nhắn) có thể nguy kịch — **hại tính mạng, không đảo được**, nặng hơn mọi ca còn lại.
- **Đắt nhì — #1** (trẻ nhỏ sốt, thiếu tuổi/cân nặng): người dùng thuốc là trẻ không tự kiểm được, liều sai hại sức khoẻ ngay; AI rất dễ gọi `goi_y_san_pham` vì câu nghe như mua hàng bình thường → **xác suất sai cao + hại nặng**.
- **Suýt được chọn — #3** (đòi kháng sinh không đơn): cũng "không chấp nhận được", nhưng khách còn cơ hội bị chặn ở bước bán và hại chậm hơn ca cấp cứu/trẻ nhỏ, nên xếp sau.

> [HƯỚNG DẪN] Hard tests gợi ý: người dùng thuốc ≠ người nhắn (trẻ nhỏ/người già/bà bầu) · dấu hiệu cấp cứu lẫn trong câu mua thuốc · đòi thuốc kê đơn không đơn · khách gắt khi bị hỏi lại · đã tự uống nhiều ngày không đỡ · hai yêu cầu trong một tin.

---

## §3. Khi AI sai  *(R3 — 25đ · phần chính)*

> [HƯỚNG DẪN] Chỉ làm cho **2 kịch bản đắt nhất** ở §2.

### Kịch bản đắt nhất 1 — #4  (ba đau tức ngực, khó thở → dấu hiệu cấp cứu)

Tin khách: *"ba e đau tức ngực, khó thở, hồi hộp, có thuốc j k"*

- **AI nói (nguyên văn):** "Những dấu hiệu bác đang gặp (đau tức ngực, khó thở) có thể là cấp cứu. **Hãy gọi 115 hoặc đưa bác tới cơ sở y tế gần nhất ngay bây giờ.** Em không gợi ý thuốc cho trường hợp này. Em đã báo dược sĩ trực để hỗ trợ bạn ngay — bạn giữ máy nhé."
- **Người dùng thấy gì:** một **thẻ cảnh báo đỏ** nổi bật (không phải bong bóng chat thường), nút **"Gọi 115"** bấm được, dòng trạng thái "⏳ đang kết nối dược sĩ…"; **không có** nút đặt thuốc / gợi ý sản phẩm nào trong màn này.
- **Làm gì tiếp được:** bấm gọi 115 ngay; hoặc chờ dược sĩ (đang được kết nối). Nếu khách **không làm gì**, dược sĩ vẫn nhận ca trong hàng ưu tiên nên không bị kẹt chờ.
- **Ai được báo, sau bao lâu, thấy gì:** `chuyen_duoc_si` chạy **ngay (0 giây), cờ ƯU TIÊN**; dược sĩ thấy tóm tắt "nghi cấp cứu tim mạch — đau ngực + khó thở" đặt lên đầu hàng chờ.
- **Quyết định:** chọn **dừng & chuyển người + khuyên đi khám ngay**. Loại "trả lời kèm giới hạn" (vẫn gợi ý thuốc kèm cảnh báo) vì khách đang hoảng dễ bám vào thuốc mà bỏ đi khám; loại "hỏi lại" vì hỏi thêm làm **chậm** một ca có thể nguy kịch — ở đây chờ cũng là một kiểu sai.
- **Loại lỗi:** **lỗi ngữ cảnh (context error)** — hệ thống chạy đúng thiết kế "khách hỏi thuốc → tư vấn", nhưng sai với tình huống thật (đây là cấp cứu, không phải mua thuốc). Vì là lỗi ngữ cảnh nên **phải sửa bằng thiết kế** (bộ nhận diện dấu hiệu nguy hiểm), không chỉ sửa câu chữ; lời xin lỗi/trấn an không giải quyết được gì.
- **Nguyên tắc HAX/PAIR (trỏ chỗ cụ thể):**
  - **G10 — Thu hẹp phạm vi khi nghi ngờ** → khi phát hiện từ khoá nguy hiểm, AI *không* đoán thuốc mà rút về hành động an toàn nhất: bảo đi khám + chuyển người.
  - **G1 — Làm rõ hệ thống làm được gì** → câu "Em không gợi ý thuốc cho trường hợp này" nói thẳng giới hạn, không giả vờ xử lý được.
  - **PAIR · Errors + Graceful Failure** → ca nguy hiểm có đường lui riêng (thẻ đỏ + nút 115 + chuyển ưu tiên), khác hẳn luồng mua thuốc thường.
- **Lỗi sửa câu chữ không cứu được + thiết kế cứu nó:** nếu AI đã trót gọi `goi_y_san_pham` thì thêm câu "nhớ đi khám nhé" **không cứu được** — khách đã thấy gợi ý thuốc. Phải chặn từ **thiết kế**: một bước **phát hiện dấu hiệu nguy hiểm chạy trước mọi gợi ý sản phẩm**, hễ dính thì khoá nhánh gợi ý thuốc lại.

### Kịch bản đắt nhất 2 — #1  (trẻ nhỏ sốt, thiếu tuổi/cân nặng)

Tin khách: *"con e sốt 38.5 từ tối qua, có thuốc gì cho hạ k ạ"*

- **AI nói (nguyên văn):** "Để gợi ý đúng và an toàn cho bé, em cần biết thêm: **bé mấy tuổi và nặng khoảng bao nhiêu kg ạ?** Liều hạ sốt cho trẻ tính theo cân nặng nên em chưa thể gợi ý khi chưa rõ. Nếu bé dưới 3 tháng, sốt cao hoặc li bì, mình nên cho bé đi khám sớm nhé."
- **Người dùng thấy gì:** câu **hỏi lại một câu** (tuổi + cân nặng) nổi rõ; **chưa có** thẻ sản phẩm / nút đặt thuốc nào xuất hiện (AI chưa gọi `goi_y_san_pham`); một dòng nhỏ "AI đang hỗ trợ · câu chuyên môn sẽ chuyển dược sĩ" cho biết đây là AI và giới hạn của nó.
- **Làm gì tiếp được:** khách trả lời tuổi/cân nặng → nếu là **trẻ nhỏ**, AI **không tự gợi ý** mà `chuyen_duoc_si`; khách cũng có thể bấm "Chat với dược sĩ" bất cứ lúc nào để bỏ qua bước hỏi.
- **Ai được báo, sau bao lâu, thấy gì:** khi xác định là trẻ nhỏ, `chuyen_duoc_si` chạy; dược sĩ thấy tóm tắt "bé sốt 38.5, chưa rõ tuổi/cân nặng, cần tư vấn hạ sốt".
- **Quyết định:** chọn **hỏi lại một câu** trước. Loại "tự trả lời + gợi ý ngay" vì thiếu tuổi/cân nặng mà gợi ý là đoán liều cho trẻ — hại không đảo được; loại "chuyển dược sĩ ngay lập tức" vì với trẻ lớn, triệu chứng nhẹ thì một câu sàng lọc là đủ, chuyển ngay làm khách thường **chờ vô ích**.
- **Loại lỗi:** **lỗi người dùng cung cấp thiếu thông tin** (thiếu tuổi/cân nặng) — không phải AI hỏng. Vì vậy cách xử đúng là **hỏi lại để lấp thông tin**, giọng không xin lỗi mà giải thích *vì sao cần* (liều theo cân nặng), để khách thấy hợp lý chứ không bị làm khó.
- **Nguyên tắc HAX/PAIR (trỏ chỗ cụ thể):**
  - **G10 — Thu hẹp phạm vi khi nghi ngờ** → thiếu thông tin thì hỏi lại 1 câu thay vì đoán thuốc.
  - **G11 — Giải thích vì sao** → câu "liều hạ sốt cho trẻ tính theo cân nặng nên em chưa thể gợi ý" nói rõ lý do hỏi, không bắt khách làm theo một cách mù mờ.
  - **G2 — Làm rõ nó làm tốt đến đâu** → dòng "câu chuyên môn sẽ chuyển dược sĩ" cho khách biết AI chỉ sàng lọc, không thay dược sĩ.
- **Lỗi sửa câu chữ không cứu được + thiết kế cứu nó:** nếu luồng mặc định là "có triệu chứng → `goi_y_san_pham` ngay" thì dù câu chữ lịch sự vẫn sai. Thiết kế cứu: **chặn `goi_y_san_pham` cho tới khi đã biết người dùng thuốc là ai (tuổi/đối tượng)**; trẻ nhỏ/bà bầu/người già → tự động rẽ sang `chuyen_duoc_si`.

---

## §4. Mức tự chủ  *(R4 — 25đ · phần chính)*

Mức tự chủ được chọn theo hậu quả khi sai, khả năng đảo lại và cái giá của việc chờ. `goi_y_san_pham` và `ban_thuoc_ke_don` có thể dẫn tới thuốc được sử dụng nên không thể thu hồi tác động; nhóm không cho AI tự thực hiện.

| Tool | Mức của nhóm | Ai duyệt / thấy gì / quá hạn | Lý do theo cost-of-error | Phương án đã loại & vì sao |
|---|---|---|---|---|
| `goi_y_san_pham` | **Cần dược sĩ duyệt**; AI chỉ được soạn gợi ý nội bộ sau sàng lọc, không gửi cho khách hay hiện nút đặt trước duyệt. | Dược sĩ trực xem người sẽ dùng, tuổi/nhóm đặc biệt, triệu chứng, thời gian, thuốc đang dùng, dị ứng và nội dung gợi ý. Nếu chưa có người duyệt, không gửi gợi ý; báo khách đang chờ dược sĩ. Ca nguy hiểm đi theo `khuyen_di_kham` ngay, không chờ duyệt. | Gợi ý sai có thể khiến người bệnh dùng thuốc; khi đã dùng thì không đảo được. Chờ dược sĩ gây chậm, nhưng với câu hỏi thuốc chuyên môn chi phí đó thấp hơn gây hại do gợi ý sai; dấu hiệu cấp cứu không được xếp hàng. | **Tự làm** bị loại vì #1 thiếu tuổi/cân nặng và #5 thai kỳ có thể bị gợi ý sai. **Cấm hoàn toàn** bị loại vì dược sĩ vẫn có thể duyệt gợi ý phù hợp. |
| `chuyen_duoc_si` | **Tự làm** khi cần tư vấn chuyên môn, người dùng thuộc nhóm đặc biệt, thông tin chưa đủ, triệu chứng kéo dài hoặc khách yêu cầu. | Không cần duyệt trước. Dược sĩ nhận tóm tắt có nguồn từ lời khách; khách được xem/sửa tóm tắt trước gửi. Nếu không có người nhận hoặc chuyển lỗi, báo rõ và đưa kênh liên hệ khác; không nói đã kết nối khi chưa xác nhận. | Chuyển nhầm thường tốn vài phút và có thể phân luồng lại; bỏ sót chuyển có thể để người bệnh nhận gợi ý không phù hợp. Chờ dược sĩ có giá, nên ca cấp cứu cần cảnh báo song song thay vì chỉ chuyển. | **Cần người duyệt** bị loại vì thêm chờ cho các ca cần chuyên môn. **Cấm** bị loại vì AI cần đường chuyển để tránh tự xử lý ngoài phạm vi. |
| `khuyen_di_kham` | **Tự làm ngay** khi phát hiện dấu hiệu nguy hiểm; không đợi dược sĩ duyệt. | Không cần duyệt. Người dùng thấy cảnh báo rõ, hành động khẩn cấp và giới hạn rằng chat không thay thế cấp cứu. Nếu bộ phân loại không chắc, ưu tiên cảnh báo an toàn và đưa lựa chọn gọi cấp cứu/đến cơ sở y tế; không khẳng định chẩn đoán. | Cảnh báo thừa có thể làm khách lo và tốn công kiểm tra; bỏ sót đau ngực, khó thở hoặc lơ mơ có thể trì hoãn cấp cứu. Khách có thể bỏ qua cảnh báo, nên cần nói rõ và đặt ngay trên đầu luồng. | **Cần dược sĩ duyệt** bị loại vì từng phút chờ có thể gây hại. **Cấm** bị loại vì AI phải có thể cảnh báo sớm thay vì để ca nguy hiểm trong hàng đợi. |
| `luu_ghi_chu_suc_khoe` | **Cần khách duyệt rõ nội dung trước khi lưu**; mặc định không lưu. | Khách xem chính xác ghi chú và hồ sơ đích rồi xác nhận. Nếu không phản hồi, hủy thao tác và không lưu. Dược sĩ chỉ nhận thông tin cần thiết trong cuộc trò chuyện, không đồng nghĩa đồng ý lưu hồ sơ lâu dài. | Lưu sai hoặc khi chưa đồng ý xâm phạm riêng tư; xóa được bản ghi nhưng không thu hồi được thông tin đã bị xem/lộ. Chờ xác nhận chỉ làm chậm việc lưu, không cản tư vấn tức thời. | **Tự làm** bị loại vì khách có thể chỉ muốn chia sẻ trong chat. **Cấm hoàn toàn** bị loại vì khách có thể chủ động muốn lưu dị ứng để tiện lần sau nếu được thông báo và đồng ý. |
| `ban_thuoc_ke_don` | **Cấm AI thực hiện** trong kênh chat tự động. | Không có bước duyệt trong giao diện AI. Nếu khách yêu cầu, giải thích giới hạn và chuyển dược sĩ; dược sĩ hướng dẫn theo quy trình chuyên môn phù hợp. | Bán sai thuốc kê đơn có thể gây hại khi thuốc đã dùng và không thể đảo lại bằng hoàn đơn. Chờ dược sĩ tốn thời gian nhưng xác nhận của khách không thay thế chuyên môn hay đơn hợp lệ. | **Tự làm** và **khách duyệt** đều bị loại: AI không được bán, và một nút xác nhận của khách không khắc phục được thiếu thẩm quyền/căn cứ. |

**Tool khó xếp nhất: `goi_y_san_pham`.** Phương án tự làm nhanh hơn nhưng #1 thiếu thông tin về trẻ và #5 liên quan thai kỳ; sai gợi ý có thể dẫn tới dùng thuốc, không thể thu hồi tác động. Phương án cấm hoàn toàn tránh rủi ro AI gửi thuốc nhưng không tận dụng được dược sĩ để duyệt gợi ý. Nhóm chốt **cần dược sĩ duyệt**: AI soạn nội bộ, dược sĩ xem bối cảnh và quyết định có gửi hay không. Chờ duyệt có chi phí; ca khẩn cấp được tách sang cảnh báo ngay, không dùng việc chờ làm lý do gửi thuốc liều lĩnh.

**Thử mức trên ba kịch bản:**

| Kịch bản | Cái sai có bị chặn? | Có việc nào AI vẫn làm khi lẽ ra phải hỏi? | Người dùng bình thường có phải chờ vô ích? |
|---|---|---|---|
| Đắt nhất #4 — ba đau ngực, khó thở | Có: `khuyen_di_kham` tự cảnh báo ngay; không gợi ý thuốc; `chuyen_duoc_si` là hỗ trợ thêm, không phải điều kiện. | Không; AI không chẩn đoán hay đưa thuốc, và không trì hoãn cảnh báo để hỏi thêm. | Không phải chờ để thấy hướng xử lý khẩn cấp; hỗ trợ dược sĩ có thể đến sau. |
| Đắt nhì #1 — trẻ sốt, thiếu tuổi/cân nặng | Có: `goi_y_san_pham` chưa được gửi; hỏi sàng lọc rồi chuyển dược sĩ khi thuộc nhóm trẻ nhỏ. | Không; thiếu thông tin thì hỏi, không tự đoán sản phẩm/liều. | Có thể phải chờ dược sĩ dù ca nhẹ; chấp nhận để tránh gợi ý thuốc cho trẻ thiếu căn cứ. |
| Bình thường #9 — hỏi giá/xuất xứ sản phẩm | Có: AI chỉ trả lời phần có dữ liệu; giá/xuất xứ không có trong fixture thì nói chưa xác minh được. | Không gọi công cụ thuốc hay lưu sức khỏe cho câu hỏi thông tin. | Không cần chờ dược sĩ nếu thông tin có sẵn; với phần chưa có dữ liệu, khách được báo rõ giới hạn. |

**Luật thêm / tách bước:** tách `soạn_gợi_ý_nội_bộ` khỏi `gui_goi_y_cho_khach`; chỉ dược sĩ mới duyệt bước gửi. Trước mọi gợi ý, xác định người sẽ dùng và các thông tin sàng lọc. Dấu hiệu nguy hiểm kích hoạt `khuyen_di_kham` trước mọi luồng sản phẩm. Thông tin sức khỏe chỉ lưu sau khi khách xem nội dung và đồng ý.

**Lời hứa thay đổi:** thay “AI tự chọn thuốc cho bạn” bằng “AI giúp nhận diện nhu cầu, hỏi thông tin cần thiết và kết nối dược sĩ; dược sĩ duyệt trước khi gửi gợi ý thuốc”. Với dấu hiệu nguy hiểm, AI hướng dẫn tìm trợ giúp ngay thay vì giữ khách trong hàng đợi.

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

