# K4 — Ngày 1: Bài Tập & Phản Ánh

## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 14h00–18h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature

Gọi `call_openai` với temperature 0.0, 0.7, 1.2 và 1.8 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Hà Nội."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi? Ở mức nào phản hồi bắt đầu
kém mạch lạc?** (2–3 câu)

> Các phản hồi đều tương đối mạch lạc. Với temperature 1.2 và 1.8 thì có cảm giác như bị thiếu token nên nó trả lời bị ngắt quãng hơn một chút so với các temp còn lại. Riêng với temp 1.8 thì có một số câu khá lủng củng.

### Câu 1.2 — Chọn temperature cho sản phẩm

**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**

> 0 cho hợp đồng pháp lý và 1.2 cho slogan quảng cáo.
>
> - Hợp đồng pháp lý cần sự chính xác nên 0 là mức phù hợp.
> - Slogan quảng cáo cần có sự sáng tạo và mới mẻ nên mức > 0.7 là phù hợp. Chọn 1.2 theo cảm quan cá nhân.

### Câu 1.3 — Đánh đổi chi phí

Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**

> Token mỗi ngày: 20000 * 2 * 500 = 20000000
> Chi phí model lớn gpt-4o: 20000000 * 0.01 = 200$
> Chi phí model lớn gpt-4o-mini: 20000000 * 0.0006 = 12$
> Trường hợp model lớn xứng đáng: Agent tự động chỉnh sửa logic mã nguồn.
> Trường hợp model nhỏ đúng: Chatbot chăm sóc khách hàng.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona

Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích máy học (machine learning) là gì?"** nhưng hai system prompt
khác nhau:

- "Bạn là một nhà thơ, trả lời mọi thứ bằng hình ảnh ví von, tránh thuật ngữ."
- "Bạn là kỹ sư phần mềm senior, trả lời chính xác, có ví dụ code khi phù hợp."

**Hai phản hồi khác nhau như thế nào (giọng văn, độ dài, mức kỹ thuật)?
Từ đó rút ra system prompt điều khiển được những khía cạnh nào của phản hồi?**
(3–4 câu)

> So sánh 2 phản hồi:
>
> * Nhà thơ: Giọng văn bay bổng; ngắn gọn (1 đoạn); mức kỹ thuật bằng 0
> * Senior Dev: Dài hơn, giọng văn logic, tập trung vào kiến thức chuyên môn.
>
> System Prompt điều khiển:
>
> 1. Hướng giải thích: Ẩn dụ kể chuyện hay định nghĩa khoa học.
> 2. Định dạng cấu trúc: Văn bản trơn hay Markdown (bôi đậm, block code).
> 3. Bộ từ vựng: Né tránh hay bắt buộc dùng thuật ngữ chuyên môn.

### Câu 2.2 — tiktoken vs đếm từ

Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**

> Ước lượng thô (200 tokens) cao hơn thực tế (194 tokens) khoảng  3% .
> Công thức số từ / 0.75 dựa trên tiếng Anh. Tiếng Việt qua bộ mã hóa mới (GPT-4o) tối ưu hơn, tốn ít token hơn hệ số này. Do đó, chi phí thực tế sẽ luôn rẻ hơn dự tính.
>
> ---

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming

**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)

> Trong ba ứng dụng, **chatbot văn bản hưởng lợi nhiều nhất** từ streaming nhờ khả năng hiển thị chữ ngay lập tức, loại bỏ cảm giác chờ đợi và tối ưu hóa trải nghiệm thời gian thực cho người dùng. Ngược lại, **pipeline dịch tài liệu chạy ngầm ban đêm hoàn toàn không cần** đến tính năng này vì đây là tác vụ xử lý hàng loạt (batch processing), vận hành tự động và không có người dùng trực tiếp ngồi đợi kết quả. Đối với trường hợp **trợ lý giọng nói**, streaming có thể mang lại lợi ích nếu hệ thống chuyển văn bản thành giọng nói (TTS) hỗ trợ stream âm thanh, tuy nhiên việc này vẫn không tối ưu bằng chatbot văn bản vì âm thanh luôn cần độ liền mạch của cả câu để đảm bảo ngắt nghỉ tự nhiên.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?

**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**

> - So với delay cố định: Exponential backoff (giãn cách thời gian tăng dần theo cấp số nhân) giúp giảm áp lực dồn dập lên máy chủ. Thay vì hàng nghìn client liên tục bắn phá API sau mỗi X giây cố định, khoảng cách thời gian giãn rộng ra giúp hệ thống có khoảng thở để tự phục hồi.
> - Tác dụng của Jitter: Kỹ thuật thêm độ trễ ngẫu nhiên (jitter) giải quyết triệt để hiện tượng thắt nút cổ chai (Thundering Herd). Nếu không có jitter, các client lỗi cùng một thời điểm sẽ tính ra thời gian backoff giống hệt nhau và đồng loạt gửi lại yêu cầu cùng lúc. Jitter làm phân tán dòng lượng truy cập này rải rác theo thời gian, giúp phân tải mượt mà cho server.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona

**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**

> System Prompt"Bạn là một giáo viên IT giảng dạy cho trẻ em. Hãy giải thích mọi khái niệm bằng ngôn ngữ siêu đơn giản và luôn kết thúc câu trả lời bằng một câu đố vui."
>
> - Xóa cụm "giảng dạy cho trẻ em/ngôn ngữ siêu đơn giản": Trợ lý sẽ quay lại trả lời theo kiểu học thuật, khô khan, chứa nhiều thuật ngữ kỹ thuật khó hiểu với người mới.
> - Xóa cụm "luôn kết thúc bằng một câu đố vui": Trợ lý sẽ chỉ dừng lại sau khi giải thích xong, làm mất tính tương tác chủ động và yếu tố vui nhộn định hình phong cách.

### Câu 4.2 — Hạn chế & cải thiện

**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**

> - Tình huống mất ngữ cảnh: Người dùng chơi trò giải đố hoặc phỏng vấn thử việc. Ở lượt chat thứ 1, người dùng nêu 3 quy tắc/yêu cầu. Đến lượt thứ 5 (sau 4 lượt hỏi đáp), toàn bộ quy tắc ban đầu bị cắt khỏi history. Trợ lý sẽ vi phạm luật chơi hoặc hỏi lại thông tin đã biết.
> - Giải pháp: Áp dụng kỹ thuật Tóm tắt lịch sử (Conversation Summary). Khi history vượt quá 4 lượt, thay vì xóa bỏ, dùng một model nhỏ (như GPT-4o-mini) tóm tắt các ý chính của các lượt cũ thành một đoạn văn ngắn và đính kèm cố định ngay sau System Prompt.

---

## Danh Sách Kiểm Tra Nộp Bài

- [X] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [X] Cả 4 checkpoint pytest đều pass
- [X] Tất cả 9 câu trong file này đã được trả lời
- [X] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
