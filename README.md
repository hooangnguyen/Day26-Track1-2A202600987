# Day17-2A202601008-DoVietAnh — Main Studio (AI Prototyping & MVP)

## Thông tin học viên
- **Trường:** VinUni
- **Mã học viên:** 2A202601008
- **Họ tên:** Đỗ Việt Anh
- **Dự án đang làm:** ISA — Trợ lý AI hỗ trợ Sinh viên Quốc tế & Hòa nhập (AI20K-C2-HE-14)
- **Vai trò:** Trưởng nhóm · Frontend/UI-UX + GitHub/CI

### Build-up Chain
| Hạng mục | Link / ghi chú |
|---|---|
| Day 16 artifact 02 (JTBD) | Lab 2 — JTBD Project Analysis (repo Day16) |
| Present board | [`assets/present-board.html`](./assets/present-board.html) — slide deck điều hướng ← → |

---

## Bước 0 — JTBD Checkpoint
| Câu hỏi | Trả lời |
|---|---|
| **Core JTBD** | Hoàn tất đúng một thủ tục cư trú/học vụ đúng hạn ngay khi cần, dù chưa thạo tiếng Việt và phải tự xoay ngoài giờ hỗ trợ. |
| **Job executor** | SV quốc tế năm nhất (<3 tháng tại VN, chưa thạo tiếng Việt). |
| **Current workflow** | Google dịch + lục web/PDF → hỏi ChatGPT → hỏi nhóm FB/bạn bè → chờ giờ hành chính hỏi Phòng CTSV. |
| **Pain step đau nhất** | **Locate** (tìm đúng nguồn chính thức, cập nhật) + **Confirm** (xác nhận hiểu đúng, đáng tin trước khi hành động). |
| **AI leverage point** | Bước **Confirm** — trả lời bám tài liệu chính thức **có trích dẫn** + guardrail/escalation. |
| **Điểm mơ hồ nhất** | Liệu trích dẫn có *thật sự* làm SV tin & rời ChatGPT không → chính là hypothesis test hôm nay. |

---

## Bước 1 — Hypothesis Card
- **Hypothesis (A3):** SV quốc tế năm nhất sẽ **tin & chuyển từ ChatGPT/Google dịch sang ISA** *vì* câu trả lời bám tài liệu chính thức **có trích dẫn nguồn** (kèm `updated_at`) và biết khi nào chuyển người thật.
- **Thuộc phần nào của dự án:** giá trị cốt lõi / khác biệt (F1 RAG có trích dẫn + F2 guardrail/escalation).
- **Nếu sai thì sập gì:** định vị "đáng tin hơn ChatGPT" sập → ISA chỉ còn là wrapper RAG chậm & hẹp hơn ChatGPT; phần lớn F1–F6 mất ý nghĩa.
- **Vì sao chọn hôm nay:** là assumption nguy hiểm nhất ở Lab 2; là value/trust risk nên nghĩ được nhiều cách test rẻ; bám đúng pain step Confirm.

---

## Bước 2 — Current Approach
- **Đã/đang làm để test:** build thẳng hệ thống thật — chatbot RAG trả lời kèm chip trích dẫn + `updated_at` (F1), guardrail + escalation (F2), nút feedback 👍👎; validate **gián tiếp** qua eval ≥40 câu (US-014), log, retention, hài lòng demo ≥8/10.
- **Cần build lớn không:** có — gần như toàn bộ sản phẩm (React FE + FastAPI/LangGraph + auth + admin KB + eval).
- **Tốn gì:** ~6 tuần · 2 người · full FE+BE · gom & curate KB · LLM API + vector store + deploy.
- **Vì sao đắt/chậm:** phải dựng xong cả pipeline mới có cái để đo, mà lại đo gián tiếp. Eval đo *groundedness/accuracy* (feasibility) — **không** trực tiếp đo "SV có **tin** và **rời ChatGPT** không" (đúng cái A3 nói).
- **Câu tự ép hỏi lại:** mình có cần app chạy thật mới biết trích dẫn có làm SV tin & đổi không, hay test được *trước khi* build?

---

## Bước 3 — Ít nhất 3 cách rẻ hơn để test cùng A3

### Cách A — So sánh tĩnh ChatGPT vs ISA (Value/Trust)
- **Loại artifact:** mockup tĩnh 1 màn (2 cột) + script phỏng vấn.
- **Người dùng thấy:** cùng 1 câu hỏi, 2 câu trả lời cạnh nhau — ChatGPT (không nguồn) vs ISA (có thẻ trích dẫn + nút xem nguồn).
- **Phía sau làm gì:** soạn sẵn 2 câu trả lời; hỏi 5 SV tin/chọn/dám-hành-động theo cột nào.
- **Rẻ hơn vì:** 0 code, không RAG, không deploy. **Học được:** trích dẫn có dịch chuyển niềm tin/ý định không. **Chưa học được:** hành vi lâu dài, độ chính xác thật.

### Cách B — Wizard-of-Oz (Behavior thật) — *teammate*
- **Loại artifact:** log chat thật (front + back-stage) chụp từ app ISA.
- **Người dùng thấy:** trợ lý trả lời câu hỏi thủ tục **kèm link nguồn** như thể bot chạy.
- **Phía sau làm gì:** người trực tra văn bản chính thức rồi trả lời tay qua hộp thư hỗ trợ.
- **Rẻ hơn vì:** 0 code, dùng kênh chat sẵn; test đúng hành vi thật. **Học được:** SV có quay lại, tin hơn ChatGPT không. **Chưa học được:** scale/độ trễ tự động.

### Cách C — Clickable prototype (Usability/Comprehension)
- **Loại artifact:** click-through prototype HTML, 4 màn + màn "🧪 Test A3".
- **Người dùng thấy:** màn chat ISA với câu trả lời + thẻ trích dẫn inline + banner guardrail + nút escalate.
- **Phía sau làm gì:** frontend giả, không backend; cho 3–5 SV thao tác.
- **Rẻ hơn vì:** không RAG/server. **Học được:** SV có hiểu & dùng được thẻ trích dẫn/escalation không. **Chưa học được:** độ chính xác nội dung, hành vi lặp lại.

---

## Bước 4 — Artifact cho cả 3 cách

### Cách A — So sánh tĩnh (file tương tác: [`assets/cach-a-mockup.html`](./assets/cach-a-mockup.html))
![Cách A — so sánh ChatGPT vs ISA](./assets/cach-a-preview.png)

### Cách B — Wizard-of-Oz (log chat thật)
Front-stage (SV thấy ISA trả lời kèm nguồn):
![Cách B front-stage](./assets/logchat.webp)

Back-stage (người trực trả lời tay qua hộp thư hỗ trợ):
![Cách B back-stage 1](./assets/logchat1.webp)
![Cách B back-stage 2](./assets/logchat3.webp)

### Cách C — Clickable prototype (file tương tác: [`assets/cach-c-mockup.html`](./assets/cach-c-mockup.html))
Câu trả lời có trích dẫn inline + nút escalate (chỉ hiện ở câu hỏi pháp lý):
![Cách C — chat](./assets/cach-c-mockup-chat-preview.png)

Màn "🧪 Test A3" — script đo comprehension:
![Cách C — test page](./assets/cach-c-mockup-testpage-preview.png)

---

## Bước 5 — So sánh nhanh 3 cách
| Tiêu chí | Cách A (tĩnh) | Cách B (WoZ) | Cách C (prototype) |
|---|---|---|---|
| Nhanh hơn ở đâu | làm trong vài giờ | 0 code, kênh có sẵn | dựng UI giả, không BE |
| Rẻ hơn ở đâu | chỉ mockup tĩnh | chỉ tốn người trực | không RAG/server |
| Gần hành vi thật | Thấp (ý kiến) | **Cao nhất** (hành vi thật) | Trung bình (thao tác giả) |
| Học rõ nhất điều gì | trích dẫn có dịch chuyển niềm tin? | SV có quay lại & tin thật? | SV có hiểu & dùng được trích dẫn/escalation? |
| Giới hạn lớn nhất | chỉ là ý kiến, dễ bias bởi cách bày 2 cột | tốn người, không scale | không đo chính xác nội dung & hành vi lâu dài |

**Chốt nhanh:**
- **Thuyết phục nhất khi present:** Cách B — là hành vi thật, có log thật từ app.
- **Dễ bị phản biện nhất:** Cách A — chỉ là ý kiến, kết quả phụ thuộc cách trình bày 2 cột (bias).
- 3 cách bổ trợ theo trục *gần hành vi thật*: A (ý kiến) → C (tương tác giả) → B (hành vi thật).

---

## Bước 6 — Present Board
Slide deck điều hướng ← →: [`assets/present-board.html`](./assets/present-board.html)

![Present board — cover](./assets/present-board-preview.png)

Board gồm 7 slide: Cover → Hypothesis → Current approach → Cách A → Cách B → Cách C → So sánh trade-off.
- **Ảnh quan trọng nhất khi present:** `logchat.webp` (B, hành vi thật) + `cach-c-mockup-chat-preview.png` (C, trích dẫn inline).

---

## Bước 7 — Present (3–4 phút)
| Phần | Thời lượng | Nội dung |
|---|---|---|
| 1 | 20–30s | JTBD checkpoint: lát cắt SV quốc tế năm nhất |
| 2 | 20–30s | Hypothesis A3 + vì sao chọn |
| 3 | 30–45s | Current approach đang đắt/chậm ở đâu |
| 4 | 90s | đi qua 3 cách test + cho xem artifact |
| 5 | 30–45s | so sánh trade-off |
| 6 | 20–30s | muốn lớp phản biện vào đâu |

- **Muốn lớp phản biện nhất vào:** _(điền sau present)_
- **Câu mở đầu 30s:** _(điền sau present)_

---

## Bước 8 — Sau Present
- Phản biện quan trọng nhất nhận được: _(điền sau present)_
- Cách test bị hỏi nhiều nhất: _(điền sau present)_
- Cần chỉnh gì trên board/artifact: _(điền sau present)_
- Điều muốn giữ nguyên sau phản biện: _(điền sau present)_

---

## Checklist
- [x] JTBD checkpoint
- [x] 1 hypothesis quan trọng (A3)
- [x] Current approach hiện tại của dự án
- [x] ≥3 cách rẻ hơn để test cùng hypothesis
- [x] Artifact cho cả 3 cách
- [x] Present board
- [ ] Present cá nhân + ghi post-notes (Bước 7–8)
