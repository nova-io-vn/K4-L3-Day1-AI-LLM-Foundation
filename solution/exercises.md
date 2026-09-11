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
> *Khi tôi tăng temperature từ 0.0 -> 1.5 thì thấy sự thay đổi về cách model trả lời, 0.0 thì câu trả lời sẽ bị gò bó, 1.5 thì câu trả lời trở nên có chiều sâu và ngôn ngữ linh hoạt hơn*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tôi sẽ chọn 0.3 vì ở mức độ này model có thể trả lời với khách hàng ổn định, tránh bịa chuyện*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *Có 10.000 người dùng/ngày, mỗi người gọi API 3 lần nên tổng cộng có khoảng 30.000 requests/ngày. Với 350 token output/request, tổng output khoảng 10,5 triệu token/ngày. GPT-4o có chi phí cao hơn đáng kể so với GPT-4o-mini, nhưng số lần chính xác phụ thuộc vào bảng giá input/output của phiên bản model đang sử dụng. GPT-4o phù hợp với các tác vụ phức tạp như reasoning, phân tích chuyên sâu hoặc yêu cầu chất lượng cao; GPT-4o-mini phù hợp với chatbot CSKH, phân loại, tóm tắt hoặc các tác vụ có volume lớn và yêu cầu chi phí thấp.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *System prompt thay đổi rõ cách model trả lời. Prompt cho trẻ em tạo câu trả lời đơn giản, ít thuật ngữ và nhiều ví dụ; prompt chuyên gia tạo câu trả lời chuyên sâu và nhiều thuật ngữ kỹ thuật. System prompt chủ yếu định hướng hành vi, phong cách và mức độ chi tiết của model.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *Số token thực tế bằng tiktoken thường khác đáng kể so với số từ / 0.75. Vì tokenizer chia văn bản thành token/subword chứ không đơn giản theo từ, đặc biệt tiếng Việt có Unicode, dấu và cách tách từ khác tiếng Anh.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming phù hợp với chatbot và nội dung dài vì người dùng thấy kết quả ngay khi model đang sinh. Non-streaming phù hợp khi ứng dụng cần toàn bộ response trước khi xử lý tiếp.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff tăng dần thời gian chờ như 1s → 2s → 4s, giúp giảm tải khi API quá tải. Nếu hàng nghìn client cùng retry sau đúng 1 giây, hệ thống có thể tiếp tục bị quá tải do các request dồn cùng lúc.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Tôi chọn persona trợ lý AI hỗ trợ học lập trình.
System prompt:
Bạn là trợ lý AI hỗ trợ sinh viên học lập trình.
Hãy giải thích bằng tiếng Việt, ngắn gọn, dễ hiểu và có ví dụ thực tế.
Nếu không chắc chắn, hãy nói rõ thay vì tự bịa thông tin.
Tôi yêu cầu trả lời ngắn gọn để dễ học và chỉ định tiếng Việt để phù hợp với người dùng.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Hạn chế lớn nhất là history hội thoại dài có thể làm tăng số token và chi phí. Tôi sẽ dùng summarization để tóm tắt các đoạn hội thoại cũ và chỉ gửi context cần thiết cho model.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
