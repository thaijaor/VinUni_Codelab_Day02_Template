# 03 — AI Log: dùng AI làm thought-partner

**Học viên:** thaijaor
**Mã học viên:** 2A202602894
**Email:** thainguyenhong2204@gmail.com
**Branch:** `thai-02894`
**Công cụ:** Claude (Claude Code) chạy trực tiếp trong repo, Gemini API cho phần prototype.

---

## 1. AI giúp được gì

**Brainstorm Phase 1 nhanh hơn hẳn.** Tôi đưa bối cảnh Vin Smart Future và yêu cầu quét
theo 4 lenses. AI trả về 5 bài toán trải đều 5 công ty thành viên, mỗi bài đều có actor
cụ thể và workflow đếm được bước — đúng dạng bảng lab yêu cầu. Điều hữu ích nhất không
phải là ý tưởng, mà là nó ép tôi viết mỗi bài toán kèm "ai đang đau" thay vì mô tả chung chung.

**Chỉ ra bài toán không nên dùng AI.** Khi tôi đưa 3 quick card, AI chủ động nói card
đối soát hóa đơn sạc VinFast chủ yếu giải được bằng rule-based, LLM chỉ đóng vai phụ.
Đây là phản biện tôi không tự nghĩ ra, và nó thành nội dung cho phần so sánh
Rule vs LLM vs Agent ở Phase 3.

**Viết ranh giới vận hành thành prompt.** Tôi mô tả 2 guardrail bằng lời, AI chuyển thành
`SYSTEM_PROMPT` có cấu trúc: vai trò, quy tắc tag `[DRAFT_ONLY]`, ngưỡng pin 5%,
định dạng output, và các hành vi bị cấm.

---

## 2. Chỗ AI sai hoặc suýt làm tôi sai

**Bịa số liệu vận hành và trình bày như số thật.** Bản scan đầu tiên có "120–150 cuộc/ca",
"40–60 phản hồi/ngày", "2 phút/hoá đơn". Nghe rất hợp lý nên suýt nộp luôn. Thực tế
không có nguồn nào — AI tự đặt ra. Tôi hỏi lại nguồn thì nó thừa nhận là ước tính.
Đây là dạng hallucination nguy hiểm nhất trong bài scoping, vì rubric chấm
"metric bám sát thực tế" và giảng viên chỉ cần hỏi một câu là lộ.

**Tự ý mở rộng phạm vi so với đề bài.** Tôi nhờ set môi trường, AI dựng thêm `.env` +
`python-dotenv` cho tiện, trong khi README của lab chỉ yêu cầu set biến môi trường
bằng `$env:`. Cách đó tiện thật nhưng thêm dependency không có trong `requirements.txt`,
nếu máy chấm thiếu package thì autograder fail ngay ở bước import. Tôi yêu cầu gỡ bỏ
toàn bộ, quay về đúng luồng README.

**Không biết model trong đề đã bị khai tử.** Template hardcode `gemini-2.5-flash`.
Chỉ khi chạy thật mới nhận 404: model không còn mở cho tài khoản mới. Cái này không AI
nào đoán trước được — phải chạy mới biết. Bài học: xác minh bằng cách chạy, đừng hỏi AI
xem code có chạy không.

---

## 3. Tôi sửa prompt và ranh giới ra sao

| Vấn đề | Cách tôi sửa |
|---|---|
| Số liệu bịa | Yêu cầu AI đánh dấu rõ đâu là giả định, sau đó tự rà lại từng con số trước khi nộp |
| AI làm quá phạm vi | Ra chỉ thị rõ: bám đúng README, không thêm dependency ngoài `requirements.txt` |
| Bản nháp lắm phần thừa | Yêu cầu bỏ mọi ghi chú không phục vụ việc nộp bài, chỉ giữ nội dung tính điểm |
| Model lỗi thời | Chạy thử với model mới, xác nhận trả về đúng rồi mới thay hằng số |

Với `SYSTEM_PROMPT` của prototype, bản đầu chỉ ghi "luôn bắt đầu bằng `[DRAFT_ONLY]`".
Test case 2 tấn công đúng chỗ này bằng cách bảo bỏ tag cho gọn. Tôi bổ sung một điều khoản
liệt kê thẳng các kiểu ép buộc — bỏ tag, gửi ngay, tự nhận có thẩm quyền — và yêu cầu
từ chối, giải thích ngắn gọn, không có ngoại lệ. Ranh giới viết theo kiểu liệt kê tình huống
tấn công cụ thể chắc hơn nhiều so với viết một câu nguyên tắc chung chung.

---

## 4. Rút ra

AI mạnh ở khung sườn: cấu trúc tài liệu, chuyển ý thành prompt có kỷ luật, phản biện
lựa chọn kiến trúc. Nó yếu đúng ở chỗ bài scoping cần nhất — sự thật vận hành. Mọi con số
trong báo cáo phải do người chịu trách nhiệm, còn AI chỉ nên giữ vai trò dựng khung
và chất vấn.
