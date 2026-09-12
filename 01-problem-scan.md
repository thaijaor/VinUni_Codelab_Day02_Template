# Lab 02 — Problem Scan & Quick Assessment

## Thông tin người thực hiện

- **Tên nhóm:** `[BỔ SUNG TÊN NHÓM]`
- **Họ và tên:** Nguyễn Cường
- **Email:** cuongnmhe181490@fpt.edu.vn
- **Branch cá nhân:** `cuong-02650`
- **Các thành viên khác:** `[BỔ SUNG HỌ TÊN VÀ EMAIL CỦA TỪNG THÀNH VIÊN]`

> **Lưu ý về số liệu:** Các mốc thời gian và chỉ tiêu dưới đây là giả định ban đầu phục vụ scoping. Nhóm cần đo baseline từ dữ liệu vận hành hoặc phỏng vấn stakeholder trước khi dùng để ra quyết định triển khai.

---

# Phase 1 — SCAN

## Danh sách bài toán vận hành

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---:|---|---|---|
| 1 | Vinhomes | Lặp lại (Repetitive) | Nhân viên phải đọc từng phản ánh của cư dân, xác định mức độ ưu tiên và chuyển thủ công đến đúng ban quản lý hoặc bộ phận kỹ thuật. Yêu cầu bị chuyển sai làm tăng thời gian xử lý và khiến cư dân phải chờ. |
| 2 | Xanh SM | Pain từ người khác (Stakeholder Pain) | Bộ phận vận hành phải đọc ghi chú của tài xế và nội dung chăm sóc khách hàng để phân loại nguyên nhân hủy chuyến. Dữ liệu không đồng nhất khiến việc tổng hợp nguyên nhân và đề xuất cải thiện dịch vụ bị chậm. |
| 3 | VinUni | AI có thể tốt hơn (AI-upgrade) | Autograder xác định test thất bại nhưng phản hồi kỹ thuật thường ngắn và khó hiểu. Trợ giảng phải đọc log, tìm nguyên nhân và soạn giải thích riêng cho nhiều sinh viên. |
| 4 | VinFast | Lặp lại (Repetitive) | Nhân viên tài chính phải đối chiếu dữ liệu phiên sạc từ nhiều trạm với hóa đơn đối tác. Khác biệt về mã giao dịch, thời gian hoặc số tiền cần được phát hiện và chuyển cho người phụ trách kiểm tra. |
| 5 | Vinpearl | Tốn thời gian (Time-consuming) | Nhân viên trải nghiệm khách hàng phải đọc đánh giá từ nhiều kênh để tìm các phàn nàn nghiêm trọng, phân loại chủ đề và chuyển trường hợp khẩn cấp đến quản lý cơ sở. |

## Ba bài toán được chọn để Quick-Assess

Ba bài toán được chọn là:

1. Vinhomes — Phân loại và điều hướng phản ánh cư dân.
2. Xanh SM — Phân loại nguyên nhân hủy chuyến.
3. VinUni — Phân tích lỗi bài lab và soạn phản hồi cho sinh viên.

Các bài toán này có đầu vào ngôn ngữ tự nhiên, workflow và actor tương đối rõ, có thể đo thời gian xử lý, đồng thời cho phép thiết kế ranh giới giữa đề xuất của AI và quyết định của con người.

---

# Phase 2 — QUICK-ASSESS

## Quick Problem Card #1 — Vinhomes

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Rút ngắn thời gian phân loại và chuyển phản ánh của cư dân đến đúng bộ phận xử lý. |
| **Công ty thành viên** | Vinhomes |
| **Actor đang gặp khó khăn** | Nhân viên tiếp nhận phản ánh, ban quản lý tòa nhà, bộ phận kỹ thuật và cư dân chờ xử lý. |
| **Workflow thủ công hiện tại** | 1. Cư dân gửi phản ánh trên ứng dụng hoặc qua tổng đài → 2. Nhân viên đọc nội dung và kiểm tra thông tin → 3. Nhân viên xác định nhóm vấn đề và mức độ ưu tiên → 4. Chuyển yêu cầu đến ban quản lý/bộ phận kỹ thuật → 5. Chuyển lại nếu phân loại sai. |
| **Bottleneck** | Bước đọc, phân loại và chọn bộ phận xử lý. Giả định ban đầu: khoảng 3–7 phút/yêu cầu, chưa tính thời gian chuyển lại khi sai. |
| **AI hỗ trợ ở đâu** | LLM trích xuất tòa nhà, loại sự cố và mức độ khẩn cấp; rule kiểm tra các trường bắt buộc; hệ thống đề xuất bộ phận tiếp nhận để nhân viên duyệt. |
| **Success Metric đề xuất** | Ít nhất 90% yêu cầu được chuyển đúng bộ phận ngay lần đầu; 90% yêu cầu có đề xuất phân loại trong dưới 30 giây; tỷ lệ phải chuyển lại dưới 5%. |
| **Quick Architecture** | **LLM Feature** kết hợp rule-based validation và Human-in-the-loop. |
| **Operational Boundary sơ bộ** | AI chỉ được phân loại và đề xuất tuyến xử lý, không tự đóng khiếu nại, không cam kết thời gian hoàn thành và không tự gửi phản hồi cuối cùng cho cư dân. Các trường hợp khẩn cấp hoặc độ tin cậy thấp phải chuyển cho nhân viên. |

### Đánh giá nhanh

- **Giá trị:** Giảm thao tác đọc và chuyển yêu cầu lặp lại, đồng thời giảm số lượt chuyển sai.
- **Khả thi:** Có thể bắt đầu bằng taxonomy nhỏ gồm các nhóm điện, nước, thang máy, an ninh, vệ sinh và tiếng ồn.
- **Rủi ro chính:** Nội dung thiếu địa điểm hoặc mô tả mơ hồ có thể khiến AI phân loại sai mức độ khẩn cấp.
- **Kết luận sơ bộ:** Phù hợp nhất để nhóm tiếp tục Deep Dive.

---

## Quick Problem Card #2 — Xanh SM

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Tự động hỗ trợ phân loại nguyên nhân hủy chuyến từ ghi chú của tài xế và nội dung chăm sóc khách hàng. |
| **Công ty thành viên** | Xanh SM |
| **Actor đang gặp khó khăn** | Chuyên viên vận hành, bộ phận chăm sóc khách hàng và quản lý chất lượng dịch vụ. |
| **Workflow thủ công hiện tại** | 1. Hệ thống ghi nhận chuyến bị hủy → 2. Nhân viên thu thập ghi chú tài xế và nội dung trao đổi với khách → 3. Đọc từng trường hợp → 4. Gán một mã nguyên nhân → 5. Tổng hợp báo cáo và chuyển các trường hợp bất thường cho quản lý. |
| **Bottleneck** | Bước đọc và chuẩn hóa nguyên nhân từ nội dung tự do. Giả định ban đầu: khoảng 5–10 phút/trường hợp nếu phải đối chiếu nhiều nguồn. |
| **AI hỗ trợ ở đâu** | LLM tóm tắt sự kiện, đề xuất một nguyên nhân trong taxonomy được duyệt và nêu bằng chứng từ nội dung đầu vào; nhân viên xác nhận hoặc sửa nhãn. |
| **Success Metric đề xuất** | Độ chính xác phân loại tối thiểu 85% trên tập dữ liệu đã được con người gán nhãn; 90% trường hợp có đề xuất trong dưới 60 giây; giảm ít nhất 50% thời gian xử lý thủ công. |
| **Quick Architecture** | **LLM Feature** kết hợp taxonomy cố định và Human-in-the-loop. |
| **Operational Boundary sơ bộ** | AI không được tự kết luận lỗi thuộc về tài xế/khách hàng, không tự áp dụng hình phạt hoặc bồi thường và không dùng thông tin ngoài hồ sơ vụ việc. Trường hợp thiếu dữ liệu hoặc độ tin cậy thấp phải được đánh dấu để con người xử lý. |

### Đánh giá nhanh

- **Giá trị:** Tạo dữ liệu nguyên nhân nhất quán hơn để phân tích tỷ lệ hủy chuyến.
- **Khả thi:** Có thể thử nghiệm với taxonomy 8–10 nguyên nhân và một tập trường hợp đã gán nhãn.
- **Rủi ro chính:** Ghi chú có thể thiên lệch, thiếu bối cảnh hoặc chứa thông tin cá nhân.
- **Kết luận sơ bộ:** Có tiềm năng nhưng cần quy trình bảo vệ dữ liệu và bộ nhãn đáng tin cậy.

---

## Quick Problem Card #3 — VinUni

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Dùng kết quả autograder để soạn phản hồi dễ hiểu, giúp sinh viên xác định và sửa lỗi bài lab nhanh hơn. |
| **Công ty thành viên** | VinUni |
| **Actor đang gặp khó khăn** | Sinh viên cần hiểu lỗi; trợ giảng và giảng viên phải đọc log và giải thích lỗi lặp lại. |
| **Workflow thủ công hiện tại** | 1. Sinh viên nộp code → 2. Autograder chạy test → 3. Sinh viên hoặc trợ giảng đọc log lỗi → 4. Trợ giảng kiểm tra code và tìm nguyên nhân → 5. Soạn hướng dẫn sửa và phản hồi cho sinh viên. |
| **Bottleneck** | Bước giải thích log và xác định nguyên nhân gốc. Giả định ban đầu: khoảng 8–15 phút/bài cần trợ giảng hỗ trợ. |
| **AI hỗ trợ ở đâu** | LLM nhận code, test thất bại và log đã được giới hạn; sau đó tóm tắt lỗi, chỉ ra vùng code liên quan và soạn gợi ý theo hướng sư phạm. |
| **Success Metric đề xuất** | 90% bài lỗi nhận được phản hồi nháp trong dưới 2 phút; ít nhất 80% phản hồi được trợ giảng chấp nhận mà không cần sửa lỗi kỹ thuật nghiêm trọng; giảm 50% thời gian hỗ trợ trung bình. |
| **Quick Architecture** | **LLM Feature** dựa trên kết quả xác định của autograder. |
| **Operational Boundary sơ bộ** | AI không được tự thay đổi điểm, không được coi suy luận của mô hình là kết quả test và không được cung cấp lời giải hoàn chỉnh khi chính sách môn học cấm. Autograder vẫn là nguồn quyết định pass/fail; khiếu nại điểm phải do giảng viên xử lý. |

### Đánh giá nhanh

- **Giá trị:** Cải thiện tốc độ và chất lượng phản hồi mà không thay thế autograder.
- **Khả thi:** Đầu vào đã có cấu trúc gồm source code, test result và error log.
- **Rủi ro chính:** AI có thể suy đoán sai nguyên nhân hoặc đưa lời giải vượt quá mức hỗ trợ được phép.
- **Kết luận sơ bộ:** Khả thi nếu giới hạn AI ở vai trò soạn phản hồi và bắt buộc dựa trên bằng chứng từ test.

---

# Đề xuất lựa chọn cho Phase 3

Nhóm nên chọn bài toán **Vinhomes — Phân loại và điều hướng phản ánh cư dân** để thực hiện Deep Dive vì workflow hiện tại dễ quan sát, bottleneck rõ, metric đo được và có thể phân chia trách nhiệm hợp lý giữa LLM, rule-based validation và nhân viên vận hành.

Trước khi chốt Problem Statement, nhóm cần xác minh các giả định sau với stakeholder hoặc dữ liệu mẫu:

1. Các kênh tiếp nhận phản ánh và taxonomy đang được sử dụng.
2. Thời gian phân loại trung bình hiện tại.
3. Tỷ lệ chuyển sai bộ phận và số lượt phải chuyển lại.
4. Danh sách trường hợp khẩn cấp bắt buộc ưu tiên.
5. Dữ liệu nào được phép đưa vào hệ thống AI và thời hạn lưu trữ.

