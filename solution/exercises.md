# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Temperature 0.0 phản hồi có tính xác định tuyệt đối, câu từ gãy gọn, tập trung vào sự thật phổ biến nhất. Temperature 0.5 văn phong tự nhiên và trôi chảy hơn, câu chữ linh hoạt nhưng vẫn bám sát thông tin chuẩn xác và an toàn. Temperature 1.0 bắt đầu xuất hiện các chi tiết ít phổ biến hơn, cách diễn đạt đa dạng, giàu tính kể chuyện nhưng đôi khi bắt đầu lan man. Temperature 1.5 phản hồi mang tính sáng tạo cao, cấu trúc câu bắt đầu kém mạch lạc, và nguy cơ xuất hiện thông tin sai lệch.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Mình sẽ chọn temperature từ 0.0 đến 0.5 cho khách hàng vì chatbot hỗ trợ khách hàng cần đặt tính chính xác, nhất quán và độ tin cậy lên hàng đầu. Bởi vì temperature thấp giúp model chọn các token có xác suất cao nhất, hạn chế tối đa ảo tưởng thông tin và giữ câu trả lời chuẩn mực qua các phiên hội thoại khác nhau.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Ước tính GPT-4o đắt hơn GPT-4o-mini 17 lần tính riêng trên lượng token đầu ra.Trường hợp dùng GPT-4o khi Xử lý tác vụ suy luận phức tạp, phân tích hợp đồng/tài liệu dài, hoặc giải quyết khiếu nại nhạy cảm. Trường hợp dùng GPT-4o-mini khi định tuyến câu hỏi, trả lời FAQ đơn giản, hoặc tóm tắt hội thoại.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Phản hồi khi persona là giáo viên tiểu học thường ngắn hơn, dùng câu đơn và từ vựng phổ thông (ví dụ: so sánh blockchain với "một cuốn sổ nhật ký mà cả lớp cùng ghi và không ai được xóa"). Phản hồi khi persona là chuyên gia tài chính thường sẽ dài hơn, sử dụng thuật ngữ kỹ thuật và thường có cấu trúc liệt kê mang tính học thuật (ví dụ: sổ cái phân tán, hàm băm, cơ chế đồng thuận... ). Điều này cho thấy system_prompt không thay đổi nội dung sự thật của câu trả lời, mà định hình văn phong, độ chi tiết và từ vựng cho câu trả lời.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Token tiktoken thường cao hơn ~40–70% so với ước lượng số từ/0.75. Lý do vì tiếng Việt có dấu và là ngôn ngữ đơn âm tiết nên bị tách thành nhiều token hơn thay vì gộp 1 token như từ tiếng Anh thông dụng.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng tương tác trực tiếp với người dùng (chatbot, trợ lý ảo, coding assistant) do câu trả lời dài và người dùng cần phản hồi tức thì để biết hệ thống đang hoạt động. Non-streaming phù hợp hơn khi output cần được xử lý như một khối hoàn chỉnh trước khi dùng, chạy batch job không có người xem trực tiếp (xử lý hàng loạt dữ liệu nền) hoặc response rất ngắn thì lợi ích của streaming gần như không đáng kể so với overhead quản lý stream.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> So với delay cố định, exponential backoff có lợi thế là giãn dần khoảng cách giữa các lần thử lại, cho server thời gian phục hồi thay vì tiếp tục dội thêm request ngay khi nó đang quá tải. Nếu hàng nghìn client cùng retry với delay cố định giống nhau, tất cả sẽ đồng loạt gửi lại request đúng cùng một thời điểm, tạo ra các đợt sóng request dồn dập khiến server vốn đã quá tải càng sập nặng hơn, hệ thống có thể rơi vào vòng lặp lỗi-retry-lỗi không bao giờ hồi phục được.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Mình sẽ chọn persona là: Trợ giảng AI cho khóa học lập trình, hỗ trợ học viên mới. 
System prompt: Bạn là trợ giảng thân thiện của khóa học lập trình cho người mới bắt đầu. Trả lời ngắn gọn, dùng ví dụ code đơn giản khi cần. Luôn trả lời bằng tiếng Việt, kể cả khi câu hỏi có từ tiếng Anh xen vào. Nếu học viên hỏi điều gì đó nằm ngoài phạm vi lập trình, hãy nhẹ nhàng hướng họ quay lại chủ đề học.
Giải thích 2 lựa chọn từ ngữ trong prompt: Trả lời ngắn gọn (tối đa 4-5 câu) vì mục đích là hỗ trợ học tập tương tác, không phải viết tài liệu; câu trả lời dài dễ khiến người mới học bị ngợp thông tin và tốn thêm token/chi phí không cần thiết cho mỗi lượt. Luôn trả lời bằng tiếng Việt, kể cả khi câu hỏi có từ tiếng Anh xen vào vì học viên lập trình thường gõ lẫn thuật ngữ tiếng Anh trong câu hỏi tiếng Việt.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất: Không có bộ nhớ dài hạn dẫn đến mất ngữ cảnh các lượt trước, học viên hỏi "cái hàm lúc nãy" mà thông tin đã bị cắt thì model không hiểu. Cải thiện đề xuất: lưu bộ nhớ ra file/database giữa các phiên: sau mỗi phiên chat, trích xuất các thông tin quan trọng (tên học viên, chủ đề đã học, lỗi thường gặp) và lưu vào một file JSON hoặc database đơn giản.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
