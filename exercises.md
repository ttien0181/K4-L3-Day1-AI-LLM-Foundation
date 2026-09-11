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
> Temperature 0.0 cho kết quả xác định, gần như điên cố. 0.5 cân bằng, có độ đa dạng nhẹ. 1.0 bắt đầu linh hoạt, dùng ví dụ phong phú hơn. 1.5 rất ngẫu nhiên, có thể trả lời không liên quan. Quy luật: càng cao temperature → càng sáng tạo nhưng giảm độ tin cậy.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Đặt temperature 0.3–0.5. Chatbot hỗ trợ khách hàng cần trả lời chính xác, nhất quán. Temperature ≥0.7 gây trả lời không ổn định, sai thông tin. 0.3–0.5 giữ độ tin cậy mà vẫn tự nhiên.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> 30.000 lần × ~350 token output. GPT-4o: ~$105/ngày. GPT-4o-mini: ~$6.30/ngày. GPT-4o đắt hơn khoảng 16.7 lần. Dùng GPT-4o khi cần lập luận phức tạp (chẩn đoán, pháp lý, code chuyên sâu). Dùng mini cho tác vụ đơn giản (tóm tắt, phân loại, FAQ).

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Giáo viên tiểu học: câu trả lời ngắn, từ đơn giản, ví dụ cụ thể —ví dụ giải thích blockchain bằng "hộp tiền điện tử cho các bạn nhỏ". Chuyên gia tài chính: câu dài, thuật ngữ kỹ thuật như smart contract, hash, consensus. System prompt thiết lập vai trò, phong cách, mức độ chi tiết và từ vựng — thay đổi nó mà không đổi câu hỏi có thể thay đổi hoàn toàn kết quả.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Chênh khoảng 15–30%. Tiếng Việt có dấu thanh (huyền, sắc, nặng, hỏi, ngã, bằng) và ký tự đặc biệt (ă, â, ê, ô, ư, ơ). Tiktoken chủ yếu được train trên tiếng Anh nên không có token riêng cho các ký tự này — chúng bị tách thành nhiều token nhỏ. Tiếng Anh chỉ 26 chữ cái, mỗi chữ là 1 token; tiếng Việt có 29 chữ + dấu, làm tăng số token. Người dùng Việt Nam tốn nhiều token hơn người dùng Anh cùng độ dài câu.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng khi người dùng chờ phản hồi tức thì — chatbot hỗ trợ, agent kỹ thuật, hoặc câu trả lời dài. In từng chunk giúp người dùng cảm thấy "phản hồi nhanh", giảm chờ đợi. Non-streaming phù hợp khi cần kết quả chính xác trước khi dùng: batch processing, phân tích dữ liệu, hoặc response dùng để kiểm tra. Streaming không làm model nhanh hơn; tổng thời gian gần như không đổi, chỉ thay đổi thời điểm ký tự đầu tiên xuất hiện.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff (0.1s → 0.2s → 0.4s) giúp client giảm tần suất retry theo thời gian, từ đó giảm áp lực lên server khi quá tải. Nếu delay cố định (luôn 1s), hàng nghìn client cùng retry sau đúng 1 giây sẽ tạo thành "đợt sóng" — server vừa hồi phục lại thì bị attack tiếp bởi cả hàng nghìn request cùng lúc, dẫn đến luân phiên quá tải liên tục. Exponential backoff làm các client pha trộn thời gian retry ngẫu nhiên, giúp server có thời gian hồi phục và xử lý request một cách ổn định hơn. Trong hệ thống thật, người ta còn thêm jitter ngẫu nhiên để tránh các chu kỳ đồng bộ.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
> Chọn persona "Trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt". System prompt: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt, dùng ví dụ cụ thể khi cần, không dùng thuật ngữ quá phức tạp nếu không cần thiết."

Lựa chọn "trả lời ngắn gọn" vì người dùng thường muốn câu trả lời nhanh, không cần thông tin dài dòng. Lựa chọn "bằng tiếng Việt" vì đối tượng là người Việt Nam — nếu không chỉ định, model có thể trả lời bằng tiếng Anh hoặc混 hợp nhiều ngôn ngữ, gây khó hiểu. Dùng "trợ giảng thân thiện" thay vì "giáo viên" hoặc "chuyên gia" để tạo cảm giác gần gũi, dễ nói chuyện, phù hợp với chatbot hỗ trợ học tập.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ giữ 3 lượt (6 message) — sau đó các câu hỏi cũ bị mất, làm chatbot "quên" ngữ cảnh sớm. Đề xuất cải thiện: dùng vector database để lưu trữ các hội thoại cũ dưới dạng embedding, khi có câu hỏi mới thì truy xuất semantic similarity để lấy lại ngữ cảnh liên quan. Triển khai: mỗi khi có hội thoại mới, mã hóa thành vector và lưu vào database (ví dụ ChromaDB, Pinecone); mỗi lần chat, tìm top-k vector tương đồng với câu hỏi hiện tại và chèn vào messages. Đây là cơ chế "bộ nhớ dài hạn" thay vì bộ nhớ ngắn hạn bị giới hạn.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
