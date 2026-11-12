# Bài nộp LAB5 — T1 Y tế · Nhóm TNS

**Lớp:** L3A · **Bàn / zone:** [điền] · **Track:** T1

| Họ và tên | Mã học viên | Việc đã làm |
|---|---|---|
| [Điền] | [Điền] | [Điền đúng phần thực tế] |
| [Điền] | [Điền] | [Điền đúng phần thực tế] |
| [Điền] | [Điền] | [Điền đúng phần thực tế] |

> Bản thiết kế học tập, dữ liệu dựng; không phải dịch vụ y tế. Những chỗ ghi [điền] cần nhóm bổ sung theo thực tế.

## §1. Lát cắt & phạm vi

**Lát cắt từ track:** Trong kênh chat Hỏi dược sĩ của Pharmacity, khi khách đang nhắn hỏi “tôi (hoặc người nhà) bị X, có thuốc gì không”, AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.

Theo mô tả sản phẩm hiện tại trong track, câu chuyên môn được chuyển cho dược sĩ; thiết kế đề xuất làm rõ dấu hiệu chuyển và trạng thái khách nhìn thấy. Người dùng thuốc có thể không phải người nhắn.

**Không làm:** (1) Không tự đưa liều vì thiếu thông tin và sai liều có thể gây hại. (2) Không bán thuốc kê đơn hoặc gợi ý thuốc thay thế qua chat tự động vì quyết định chuyên môn sai có thể gây hại. (3) Không lưu thông tin sức khỏe vào hồ sơ khi chưa được khách đồng ý vì đó là thông tin nhạy cảm.

## §2. Kịch bản rủi ro

**Bốn lớp:** ① Nguồn sự thật · ② Mơ hồ/thiếu thông tin · ③ Ngoài phạm vi/thẩm quyền · ④ Đặc thù y tế. Nhãn thể hiện mức hại có thể xảy ra, không phải xác suất.

| # | Tin nhắn giả lập | Lớp | AI có thể sai — căn cứ cụ thể | Sản phẩm tốt nên làm gì | Mức hại |
|---|---|---|---|---|---|
| 1 | “Mình đau họng hai ngày, uống thuốc nào và bao nhiêu viên?” | ①③ | Tự nói liều hoặc dùng `goi_y_san_pham` dù tin mồi không có tuổi, dị ứng, thuốc đang dùng hay căn cứ về liều; track không cho AI tự nói liều. | Không tự đưa liều hay gửi gợi ý; hỏi thông tin cần thiết và chuyển dược sĩ. | Không chấp nhận được |
| 2 | “Ba tôi 78 tuổi đang uống nhiều thuốc tim mạch, mất ngủ mấy hôm nay. Có sản phẩm nào dùng được không?” | ②④ | Gọi `goi_y_san_pham` khi chưa biết các thuốc đang dùng và bệnh nền. Track yêu cầu chuyển dược sĩ với người cao tuổi, người có bệnh mạn tính hoặc đang dùng thuốc kê đơn; thuốc đã dùng không đảo lại được. | Không tự gợi ý; chuyển dược sĩ, cho khách bổ sung thông tin để dược sĩ xem xét. | Không chấp nhận được |
| 3 | “Con mình sốt, giờ khó thở, có thuốc gì không?” | ④ | Chỉ gọi `chuyen_duoc_si` hoặc `goi_y_san_pham` và để khách chờ; track nêu khó thở là dấu hiệu nguy hiểm cần xử lý ngay. | Hiện lời khuyên gọi cấp cứu hoặc đến cơ sở cấp cứu ngay; không gợi ý thuốc, không bắt chờ. | Không chấp nhận được |
| 4 | “Mình bầu ba tháng, có thuốc cảm nào chắc chắn an toàn cho em bé không?” | ①④ | Khẳng định an toàn rồi gọi `goi_y_san_pham`, dù track không cấp căn cứ xác nhận an toàn cho trường hợp này và yêu cầu chuyển dược sĩ với người mang thai. | Không khẳng định an toàn hay tự gợi ý; chuyển dược sĩ. | Không chấp nhận được |
| 5 | “Bán tôi kháng sinh này đi, tôi không có đơn.” | ③④ | Gọi `ban_thuoc_ke_don` dù track không cho bán thuốc kê đơn qua chat tự động; thuốc đã dùng thì không thể đảo lại. | Không bán tự động; chuyển dược sĩ để hướng dẫn. | Không chấp nhận được |
| 6 | “Không bán được thuốc huyết áp đó thì cho tôi thuốc khác thay thế.” | ③④ | Dùng `goi_y_san_pham` để đề xuất thuốc thay thế cho thuốc kê đơn, trái giới hạn của track; dùng sai có thể gây hại và không đảo lại được. | Không tự đề xuất thuốc thay thế; chuyển dược sĩ để xem xét. | Không chấp nhận được |
| 7 | “Tôi dị ứng thuốc này. Đừng lưu hồ sơ, chỉ tư vấn giúp thôi.” | ②③ | Gọi `luu_ghi_chu_suc_khoe` dù khách chưa đồng ý và “thuốc này” chưa được xác định; track nói xóa được hồ sơ nhưng vẫn có rủi ro lộ dữ liệu. | Không lưu vào hồ sơ; hỏi rõ khi cần để tư vấn và chỉ lưu nếu khách đồng ý với nội dung cụ thể. | Sửa được; dữ liệu đã lộ có thể không thu hồi được |
| 8 | “Còn nước muối không? Mẹ tôi đau ngực, uống gì được?” | ①④ | Chỉ trả lời hàng hóa và bỏ sót đau ngực; track nêu đau ngực là dấu hiệu nguy hiểm, đồng thời không cấp tool hay dữ liệu tồn kho. | Ưu tiên `khuyen_di_kham` ngay, không gợi ý thuốc hay bắt chờ; không tự khẳng định còn hàng. | Không chấp nhận được |

**Chọn #3 và #2.** #3 có thể trì hoãn cấp cứu, hậu quả có thể xảy ra nhanh và không đảo ngược; vì vậy được ưu tiên hơn những ca chưa nêu dấu hiệu nguy hiểm tức thời. #2 có nguy cơ nhầm người nhắn với người dùng thuốc: AI gợi ý sai có thể khiến trẻ dùng thuốc trước khi lỗi được phát hiện, và thuốc đã dùng không thể thu hồi tác động. Hai ca cho thấy hai đường xử lý khác nhau: cảnh báo ngay và chuyển dược sĩ khi cần chuyên môn.

**Ca #4 suýt được chọn:** người mang thai cũng có thể chịu hậu quả nghiêm trọng nếu AI khẳng định thuốc an toàn thiếu căn cứ. Nhóm chọn #2 để làm rõ thêm nguy cơ nhầm người dùng thuốc với người nhắn; không coi #4 là an toàn hơn trong mọi tình huống, và vẫn chuyển dược sĩ.


## §3. Khi AI sai

### Ca #2 — trẻ sốt, thiếu thông tin
- **Loại lỗi cần chặn:** lỗi ngữ cảnh nếu hệ thống giả định người nhắn là người dùng thuốc; khách không có lỗi chỉ vì chưa nói đủ. Nếu hệ thống đã gợi ý nhầm, đó là lỗi hệ thống: xin lỗi, rút gợi ý khỏi giao diện và chuyển dược sĩ; không khẳng định rút thông báo sẽ đảo được việc đã dùng thuốc.
- **Câu AI:** “Mình chưa đủ thông tin để gợi ý thuốc cho bé. Mình sẽ chuyển cuộc trò chuyện cho dược sĩ. Bạn có thể bổ sung tuổi và tình trạng của bé trong lúc chờ.”
- **Thấy/làm tiếp:** nhãn “Chưa gợi ý thuốc”, trạng thái chuyển mô phỏng, nút bổ sung/sửa thông tin và chuyển dược sĩ. Không có nút đặt thuốc. Không bổ sung thì vẫn giữ đường chuyển; không tự gợi ý.
- **Ai được báo:** dược sĩ trực nhận bản tóm tắt ngay khi khách bấm chuyển trong bản mô phỏng; hệ thống thật cần gửi qua kênh trực và xác nhận nhận ca. Không có thời gian chờ thực tế trong fixture, nên không hứa thời gian. Nếu chuyển thất bại, thông báo ngay và hướng đến liên hệ trực tiếp.
- **Quyết định:** chuyển dược sĩ, đồng thời cho bổ sung thông tin; loại tự gợi ý vì track yêu cầu trẻ nhỏ được dược sĩ xử lý; loại chỉ hỏi thêm mà không chuyển vì có thể kéo dài thời gian tiếp cận chuyên môn.

### Ca #3 — trẻ khó thở
- **Loại lỗi cần chặn:** lỗi hệ thống nếu bỏ sót “khó thở” hoặc xếp hàng trước cảnh báo; lỗi ngữ cảnh nếu chỉ đọc câu hỏi mua thuốc mà bỏ bối cảnh sức khỏe. Không đổ lỗi cho khách. Nếu bỏ sót, xin lỗi và hiện cảnh báo ngay khi nhận biết; cải thiện luồng ưu tiên chứ không chỉ đổi câu chữ.
- **Câu AI:** “Khó thở có thể là dấu hiệu nguy hiểm. Hãy gọi cấp cứu hoặc đưa bé đến cơ sở cấp cứu ngay. Đừng chờ phản hồi trong cuộc trò chuyện này.”
- **Thấy/làm tiếp:** cảnh báo nổi bật, gọi 115 hoặc tìm cơ sở cấp cứu, tùy điều kiện thực tế; nút quay lại. Không có nút đặt thuốc. Nếu khách không thao tác, cảnh báo vẫn hiển thị; prototype không tự gọi điện.
- **Ai được báo:** khách thấy cảnh báo ngay; prototype không gửi thông báo thật cho dược sĩ hay dịch vụ cấp cứu. Có nút chuyển dược sĩ mô phỏng nếu khách muốn hỗ trợ thêm, không phải điều kiện để đi cấp cứu.
- **Quyết định:** cảnh báo khẩn cấp ngay; loại hỏi đủ thông tin trước vì gây chậm; loại chỉ chuyển dược sĩ vì khách có thể chờ trong hàng. Không suy diễn chẩn đoán.

**Nguyên tắc:** G10 → [1] không gợi ý và thu hẹp phạm vi khi thiếu thông tin; G9 → [2] cho sửa thông tin trước khi gửi chuyển; PAIR Errors + Graceful Failure → [3] cảnh báo ngay và đường chuyển dược sĩ. Các số tương ứng trên prototype.

**Câu chữ không đủ:** nếu cảnh báo khó thở nhưng nút mua thuốc vẫn hoạt động, khách vẫn có thể mua nhầm. Thiết kế loại nút gợi ý/đặt thuốc khỏi cả hai ca, và đưa cảnh báo lên trước mọi câu hỏi khác.

## §4. Mức tự chủ

| Tool | Mức và điều kiện | Sai thì ai chịu / đảo lại / giá của việc chờ | Phương án loại |
|---|---|---|---|
| `goi_y_san_pham` | Cần dược sĩ duyệt trước khi gửi; dược sĩ xem đối tượng dùng, thông tin sàng lọc, nội dung gợi ý. Chưa duyệt thì không gửi. | Người dùng có thể bị hại; thuốc đã dùng không đảo lại. Chờ có thể làm khách chậm tiếp cận tư vấn. | Tự gửi bị loại vì thiếu căn cứ; cấm mọi gợi ý bị loại vì dược sĩ có thể kiểm tra. |
| `chuyen_duoc_si` | Tự làm khi cần; prototype cho khách bấm bắt đầu mô phỏng chuyển. | Chuyển thừa tốn vài phút và có thể chuyển lại; chờ lâu có thể trì hoãn hỗ trợ. | Bắt duyệt từng lần bị loại vì thêm chờ. |
| `khuyen_di_kham` | Tự làm ngay khi có dấu hiệu nguy hiểm. | Cảnh báo thừa gây lo và tốn công; bỏ sót có thể gây hại nghiêm trọng; khách có thể bỏ qua. Chờ người duyệt gây chậm. | Chờ dược sĩ duyệt bị loại vì trì hoãn. |
| `luu_ghi_chu_suc_khoe` | Khách duyệt nội dung cụ thể trước khi lưu; không đồng ý hoặc hết thời gian thì không lưu. | Lưu sai/lộ ảnh hưởng quyền riêng tư; xóa được nhưng không thu hồi được dữ liệu đã lộ. Chờ làm thông tin chưa sẵn trong hồ sơ. | Tự lưu bị loại vì không có đồng ý; cấm hoàn toàn bị loại vì khách có thể muốn lưu. |
| `ban_thuoc_ke_don` | Cấm trong chat tự động. | Sai có thể gây hại không đảo lại; chuyển chuyên môn khiến khách chờ nhưng không tạo quyền bán tự động. | Khách duyệt bị loại vì xác nhận không thay thế chuyên môn/đơn. |

**Khó xếp nhất — `goi_y_san_pham`:** tự gửi nhanh nhưng ca #2 và #4 có thể gây hại; dược sĩ duyệt giảm rủi ro nhưng làm người hỏi thông thường phải chờ; cấm mọi gợi ý giảm khả năng hỗ trợ. Nhóm đề xuất duyệt trước. Ghi nhận ý kiến thực tế của từng thành viên trước khi nộp: [thành viên 1: …; thành viên 2: …; thành viên 3: …]. Không giả định đây là cuộc thảo luận đã diễn ra.

**Thử mức:**

| Ca | Thao tác theo mức | Chặn sai? Còn tự làm khi phải hỏi? Có chờ vô ích? |
|---|---|---|
| #2 trẻ sốt | Không gửi gợi ý; chuyển dược sĩ, cho sửa/bổ sung. | Chặn gợi ý tự động trong prototype; không tự đưa thuốc. Có chờ dược sĩ, chấp nhận vì trẻ thuộc nhóm chuyển. |
| #3 khó thở | Hiện cảnh báo ngay; không gợi ý; chuyển là tùy chọn bổ sung. | Chặn luồng mua thuốc trong prototype; không đợi hỏi đủ hay chờ duyệt. Không chờ để nhận cảnh báo. |
| Bình thường: người lớn đau họng #1 | Hỏi thông tin; chuyển dược sĩ trước gợi ý. | Không tự đưa thuốc khi thiếu dữ liệu. Có chờ chuyên môn, kể cả ca nhẹ; đây là đánh đổi còn lại. |

**Tách bước đề xuất:** soạn gợi ý nội bộ tách khỏi gửi gợi ý và nút đặt; chỉ dược sĩ duyệt mới được gửi. Prototype chưa triển khai chức năng này. Lời hứa thay đổi từ “AI tự chọn thuốc cho bạn” thành “AI tiếp nhận và cảnh báo, dược sĩ xem xét trước khi gợi ý”.

## §5. Prototype

- File: [prototype/index.html](prototype/index.html). Công cụ: HTML/CSS/JavaScript chạy tại máy, không backend, không gọi AI thật. Hai ca nút chọn là kịch bản dựng; không phân tích tin nhắn tự do, không thay thế kiểm thử lâm sàng.
- [1] Nhãn “chưa gợi ý thuốc”: G10, giới hạn quyền tự làm. [2] Ô sửa thông tin: G9, khách kiểm soát nội dung mô phỏng chuyển. [3] Cảnh báo khẩn cấp và nút chuyển: PAIR, AI tự cảnh báo/chuyển mô phỏng.
- Chọn mô tả căn cứ bằng lời “trẻ nhỏ” hoặc “khó thở”, không hiển thị phần trăm độ chắc vì không có mô hình hoặc dữ liệu hiệu chuẩn. Chọn trạng thái rõ và nút hành động thay vì tự gửi sản phẩm; loại nhãn phần trăm không có căn cứ.
- **Ảnh đánh số:** [Nhóm chụp 1–2 ảnh prototype với các số 1–3 và đặt trong prototype/ trước khi nộp.] Prototype không gửi dữ liệu, không gọi cấp cứu, không lưu hồ sơ hay liên hệ dược sĩ thật.

## §6. Tốt là gì

| Tiêu chí Pass/Fail có thể kiểm tra | #2 | #3 |
|---|---|---|
| Không có gợi ý hay nút đặt thuốc trong hai ca | Pass: không hiện | Pass: không hiện |
| Người dùng thấy đường hành động phù hợp: chuyển dược sĩ ở #2; cảnh báo cấp cứu ngay ở #3 | Pass: nút chuyển mô phỏng | Pass: cảnh báo ngay khi chọn ca |
| Trạng thái nói rõ giới hạn: chưa gợi ý; chuyển/gọi thật không được giả là đã thực hiện | Pass: trạng thái mô phỏng | Pass: cảnh báo và chú thích mô phỏng |

Đây là đánh giá trên luồng dựng, không chứng minh hệ thống nhận diện mọi cách diễn đạt. Còn thiếu kết nối dược sĩ thật, phân tích tin tự do, xử lý lỗi kết nối thực và thử nghiệm với người dùng.

## §7. Demo

- Người 1 [điền]: giới thiệu lát cắt và cái giá khi sai (30 giây).
- Người 2 [điền]: chọn ca trẻ khó thở, chỉ [3], rồi chọn ca trẻ sốt, sửa thông tin tại [2] và bấm chuyển mô phỏng.
- Người 3 [điền]: giải thích `goi_y_san_pham` cần dược sĩ duyệt, `khuyen_di_kham` tự cảnh báo; chỉ [1] và trả lời ca lạ.
- Ca lạ dự kiến để luyện: “Mẹ đau ngực và muốn hỏi giá nước muối” → ưu tiên cảnh báo ngay, không chờ câu hỏi tồn kho. **Ca lạ thực tế nhận tại demo và điều chỉnh sau demo:** [điền sau khi xảy ra].
