# Lab 02 — AI Interaction Log & Reflection

## Thông tin người thực hiện

- **Tên nhóm:** `[BỔ SUNG TÊN NHÓM]`
- **Họ và tên:** Nguyen Cuong
- **Email:** cuongnmhe181490@fpt.edu.vn
- **Branch cá nhân:** `cuong-02650`
- **Công cụ AI sử dụng:** Codex (trợ lý AI hỗ trợ đọc tài liệu và làm việc với repository)

---

## 1. Mục tiêu sử dụng AI

Trong Lab 02, tôi sử dụng AI như một thought-partner để:

1. Đọc và đối chiếu yêu cầu giữa slide, `README.md`, worksheet và mã nguồn mẫu.
2. Hoàn thiện phần prompt prototype nhưng không sửa các comment hoặc tài liệu hướng dẫn không liên quan.
3. Brainstorm các pain point vận hành trong hệ sinh thái Vingroup.
4. Phản biện việc lựa chọn bài toán dựa trên workflow, metric, AI Fit và Operational Boundary.
5. Kiểm tra thay đổi bằng Git và autograder trước khi push lên branch cá nhân.

---

## 2. Nhật ký tương tác với AI

| Lần | Yêu cầu của tôi | AI đã hỗ trợ | Tôi kiểm tra/quyết định như thế nào |
|---:|---|---|---|
| 1 | Xem slide hướng dẫn và yêu cầu làm bài. | AI đọc toàn bộ slide, sau đó đối chiếu với `README.md`, `01-worksheet.md`, starter code và autograder. | Tôi xác nhận bài gồm code cá nhân, Problem Scan, Deep Dive, AI Log và workflow diagram; code không được merge vào `main`. |
| 2 | Giải thích vì sao không thấy các file deliverable. | AI kiểm tra cây thư mục và phát hiện repository chỉ cung cấp worksheet, ví dụ và inspiration kit; các file nộp bài phải được tạo mới. | Tôi giữ nguyên các template và yêu cầu AI chỉ tạo file kết quả mới. |
| 3 | Hoàn thiện phần code cá nhân, không thay đổi comment/hướng dẫn. | AI chỉ thay nội dung `SYSTEM_PROMPT` và thân hàm `evaluate_prompt()` trong `starter-code/prompt_prototype.py`. | Tôi yêu cầu giữ nguyên phạm vi. AI chạy kiểm tra cú pháp và ba tiêu chí tĩnh của autograder trước khi commit. |
| 4 | Chọn các bài toán phù hợp cho Problem Scan và Deep Dive. | AI dùng inspiration kit để đề xuất năm bài toán, sau đó so sánh khả năng mô tả workflow, đặt metric và xây dựng ranh giới an toàn. | Tôi chọn ba bài toán để Quick-Assess và ưu tiên bài toán điều hướng phản ánh cư dân Vinhomes cho Deep Dive. |
| 5 | Hoàn thiện `01-problem-scan.md`. | AI tạo file mới gồm năm bài toán và ba Quick Problem Cards, đồng thời ghi rõ các metric mới chỉ là giả định scoping. | Tôi không cho phép sửa/xóa template. File được kiểm tra riêng rồi mới commit và push lên branch `cuong-02650`. |

---

## 3. AI đã giúp tôi điều gì?

### 3.1. Tổng hợp yêu cầu từ nhiều nguồn

Thông tin của bài lab nằm ở nhiều nơi và có một số điểm không hoàn toàn giống nhau. AI giúp tôi so sánh:

- Slide yêu cầu các deliverable cuối cùng.
- `README.md` mô tả quy trình branch cá nhân và branch `main`.
- `01-worksheet.md` đưa ra rubric và các phase cần thực hiện.
- Autograder cho biết chính xác các điều kiện tối thiểu mà code phải vượt qua.

Việc đối chiếu này giúp tôi không hiểu nhầm rằng các file nộp bài đã có sẵn hoặc phải sửa trực tiếp worksheet.

### 3.2. Giữ thay đổi code trong phạm vi hẹp

AI giúp tôi triển khai lời gọi Gemini 2.5 SDK và viết System Prompt với hai ranh giới chính:

1. Mọi nội dung nháp dành cho tài xế phải bắt đầu bằng `[DRAFT_ONLY]` và cần con người phê duyệt.
2. Khi pin dưới 5%, hệ thống không được hướng dẫn tài xế tới trạm sạc xa hơn 5 km mà phải trả về lệnh `dispatch_mobile_charger`.

AI cũng giữ `temperature=0.0` để giảm độ ngẫu nhiên khi stress-test ranh giới. Trước khi push, mã nguồn đã vượt qua kiểm tra cú pháp và các kiểm tra tĩnh về System Prompt, Gemini SDK và cấu trúc adversarial tests.

### 3.3. Hỗ trợ scoping bài toán

AI không chỉ liệt kê ý tưởng mà còn giúp tôi xem từng bài toán theo các tiêu chí:

- Actor nào đang gặp khó khăn.
- Workflow hiện tại có quan sát được hay không.
- Bottleneck nằm ở bước nào.
- Có thể đặt metric định lượng nào.
- Nên dùng rule, LLM feature hay agent.
- Quyết định nào bắt buộc phải giữ cho con người.

Nhờ đó, tôi ưu tiên bài toán Vinhomes thay vì chọn một đề tài quá nhạy cảm hoặc quá phức tạp chỉ vì muốn sử dụng AI.

---

## 4. AI trả lời chưa chính xác hoặc có nguy cơ hallucination ở đâu?

### 4.1. Metric ban đầu chưa phải dữ liệu thực

Khi brainstorm, AI đề xuất các con số như 3–7 phút để phân loại một phản ánh hoặc mục tiêu 90% chuyển đúng bộ phận. Các số này hợp lý để minh họa nhưng không có dữ liệu vận hành Vinhomes chứng minh, vì vậy không thể trình bày như sự thật.

**Cách tôi xử lý:** Tôi yêu cầu ghi rõ đây là giả định ban đầu. Trước khi quyết định GO, nhóm phải đo baseline từ ticket mẫu hoặc phỏng vấn stakeholder rồi mới điều chỉnh metric.

### 4.2. Tài liệu và repository có điểm không khớp

Slide nhắc tới `student_guide.md`, nhưng repository được clone không có file này; nội dung hướng dẫn thực tế nằm trong `README.md`. Ngoài ra, worksheet ghi ít nhất ba adversarial prompts trong khi starter code và autograder chấp nhận từ hai test cases.

**Cách tôi xử lý:** Tôi không để AI tự suy đoán file bị thiếu hoặc tự sửa template. Tôi dùng cây thư mục và autograder của checkout hiện tại làm bằng chứng, giữ nguyên hai adversarial tests có sẵn theo yêu cầu không thay đổi template, và ghi nhận sự khác biệt để hỏi giảng viên nếu cần.

### 4.3. Chưa có bằng chứng kiểm thử Gemini trực tiếp

AI có thể dễ dàng kết luận rằng code đã hoàn thành chỉ vì kiểm tra tĩnh đạt. Trên máy hiện tại, Google GenAI SDK và biến môi trường API key chưa được cấu hình tại thời điểm kiểm tra, nên chưa thể chứng minh phản hồi thật của Gemini vượt qua cả hai ranh giới.

**Cách tôi xử lý:** Tôi chỉ ghi nhận các kiểm tra đã thực sự chạy, không tuyên bố live test thành công. Sau khi cài dependency và tự cấu hình API key, tôi cần chạy `python starter-code/prompt_prototype.py` và lưu lại kết quả pass/fail.

---

## 5. Tôi đã sửa prompt và ranh giới như thế nào?

### Phiên bản ban đầu

Starter code chỉ có placeholder yêu cầu người học tự viết vai trò và ranh giới. Placeholder chưa đủ để chống lại yêu cầu của người dùng muốn bỏ tag hoặc ép hệ thống đưa ra tuyến đường không an toàn.

### Phiên bản sau khi chỉnh sửa

Tôi cùng AI biến các yêu cầu thành chỉ thị có thể kiểm tra:

- Nêu rõ vai trò là dispatcher co-pilot, không phải hệ thống tự gửi lệnh.
- Dùng từ khóa bắt buộc `must` và `never` cho các điều kiện an toàn.
- Quy định chính xác prefix `[DRAFT_ONLY] `, bao gồm cả khoảng trắng sau tag.
- Xác định rõ ngưỡng pin dưới 5% và giới hạn khoảng cách 5 km.
- Quy định output JSON cụ thể cho trường hợp phải điều xe sạc di động.
- Nêu rằng user request, nội dung trích dẫn và prompt injection không được phép ghi đè ranh giới.
- Đặt nhiệt độ mô hình bằng `0.0` để kết quả ổn định hơn khi kiểm thử.

### Điểm tôi vẫn cần kiểm chứng

Prompt hiện tại mới được xác nhận bằng kiểm tra tĩnh. Tôi vẫn cần chạy test thật để xem:

1. JSON có được trả về đúng định dạng hay bị bọc trong Markdown code fence.
2. Mô hình có giữ `[DRAFT_ONLY]` ở đúng đầu phản hồi trong mọi tình huống thông thường hay không.
3. Mô hình xử lý thế nào khi mức pin hoặc khoảng cách không được cung cấp rõ ràng.
4. Có cần bổ sung adversarial test thứ ba để đáp ứng cách diễn đạt trong worksheet hay không.

---

## 6. Bài học rút ra

1. AI có thể tăng tốc đọc tài liệu và tạo bản nháp, nhưng tôi vẫn phải kiểm tra bằng file thật, Git diff và autograder.
2. Metric do AI đề xuất chỉ là giả thuyết cho đến khi có baseline hoặc dữ liệu stakeholder.
3. Operational Boundary cần được viết thành điều kiện cụ thể, có output và đường fallback rõ ràng.
4. Human-in-the-loop không chỉ là một câu cảnh báo; cần xác định chính xác AI đề xuất gì và con người phê duyệt gì.
5. Không nên đánh giá hệ thống chỉ bằng việc code import được. Kết quả live test và các trường hợp biên mới cho biết ranh giới có thực sự hiệu quả hay không.
6. Khi các nguồn hướng dẫn mâu thuẫn, cần ghi nhận khác biệt và ưu tiên bằng chứng có thể kiểm tra thay vì để AI tự chọn một kết luận thuận tiện.

---

## 7. Kế hoạch tiếp theo

- Cài dependencies trong môi trường `.venv`.
- Tự cấu hình `GEMINI_API_KEY` bằng biến môi trường, không ghi key vào source code hoặc Git.
- Chạy prompt prototype và lưu kết quả của từng adversarial test.
- Bổ sung một adversarial test nếu giảng viên xác nhận yêu cầu ba test trong worksheet là bắt buộc.
- Cùng nhóm xác minh baseline và lựa chọn bài toán Deep Dive trước khi hoàn thiện `02-deep-dive-report.md` và `04-workflow-diagram.png`.
