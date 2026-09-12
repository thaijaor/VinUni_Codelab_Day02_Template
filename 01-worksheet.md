# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên tiếp nhận và xử lý thủ công các báo cáo khẩn cấp từ tài xế taxi điện về sự cố cạn pin/trạm sạc quá tải trên đường (mất 12-15 phút/lượt tra cứu định vị GPS và tìm trụ sạc trống). |
| 2 | **Vinhomes** | Lặp lại | Phân loại tự động các phản ánh kỹ thuật (sự cố điện, rò rỉ nước sinh hoạt, hỏng thang máy) của cư dân trên App Vinhomes Resident để định tuyến tới đúng ban quản lý phân khu (mất 2-4 tiếng xử lý thủ công). |
| 3 | **Vinmec** | Tốn thời gian | Bác sĩ điều trị mất 25-30 phút/bệnh nhân để tổng hợp lịch sử xét nghiệm, diễn tiến điều trị từ hệ thống bệnh án điện tử (EMR) và soạn thảo bản tóm tắt hồ sơ xuất viện (Discharge Summary). |
| 4 | **VinFast** | AI-upgrade | Tiếp nhận phản ánh mô tả tiếng Việt tự nhiên của chủ xe (ví dụ: *"xe phát ra tiếng rít phanh khi rẽ trái ở tốc độ thấp"*) để tự động nhận diện và phân loại nhóm lỗi kỹ thuật sơ bộ, gợi ý đặt lịch xưởng dịch vụ 3S. |
| 5 | **Vinpearl / VinWonders** | Pain từ người khác | Nhân viên CSKH quá tải vào mùa cao điểm vì phải trả lời hàng nghìn câu hỏi lặp lại từ du khách về lịch biểu diễn, quy định mang hành lý/đồ ăn, chiều cao an toàn trò chơi và kiểm tra tình trạng vé vào cổng. |
| 6 | **VinFast** | Lặp lại | So khớp và đối soát hóa đơn sạc điện định kỳ giữa nhật ký phiên sạc (charging session logs) tại trạm sạc VinFast với báo cáo doanh thu tài chính để phát hiện sai lệch doanh thu. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

### Quick Problem Card #1 — Xanh SM (GSM): Xử lý sự cố pin & điều phối trạm sạc thực địa

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tài xế Xanh SM gặp sự cố cạn pin/hỏng     │
│ sạc trên đường cần điều phối viên định vị, tìm trạm sạc     │
│ VinFast còn trụ trống hoặc điều xe sạc lưu động khẩn cấp.   │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế Xanh SM (lo hết pin giữa đường, │
│ áp lực trễ cuốc), Điều phối viên Dispatcher (quá tải).      │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi hotline/báo SOS trên App tài xế             │
│   ──> 2. Dispatcher tra cứu vị trí GPS & model xe (VF5/VF8) │
│   ──> 3. Tra cứu dashboard VinFast tìm trạm sạc còn trụ    │
│   ──> 4. Soạn tin nhắn hướng dẫn/lộ trình gửi tài xế        │
│   ──> 5. Gọi xe sạc lưu động nếu pin < 5% không đủ đi       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10-12 phút) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4            │
│ (Tự động tra API trạm trống, sinh nháp [DRAFT_ONLY] chỉ dẫn │
│ đường hoặc đề xuất cứu hộ sạc nếu pin dưới ngưỡng 5%).      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.    │
│ - Tỷ lệ chỉ dẫn đúng trạm sạc và cổng sạc khả dụng >= 98%.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #2 — Vinhomes: Tiếp nhận & phân loại phản ánh kỹ thuật cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tiếp nhận, phân loại mức độ khẩn cấp và   │
│ tự động định tuyến phản ánh sự cố kỹ thuật của cư dân trên   │
│ App Vinhomes Resident đến đúng ban quản lý phân khu.        │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Cư dân Vinhomes (chờ xử lý lâu, bức    │
│ xúc), CSKH/Lễ tân tòa nhà (quá tải vì gán nhãn thủ công).  │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh (văn bản/ảnh) qua App Resident     │
│   ──> 2. CSKH đọc, xác định loại sự cố (điện/nước/thang máy)│
│   ──> 3. CSKH gán nhãn ticket & forward thủ công kỹ thuật   │
│   ──> 4. CSKH soạn tin nhắn xác nhận tiếp nhận gửi cư dân   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 15-20 phút/   │
│ ticket; trễ từ lúc gửi đến khi kỹ thuật nhận mất 2-4 tiếng).│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 & 4          │
│ (LLM phân tích ngôn ngữ tự nhiên, trích xuất thực thể, gán  │
│ nhãn mức độ ưu tiên, route ticket và draft phản hồi).       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Giảm thời gian định tuyến ticket từ 2 tiếng ──> dưới 1 min│
│ - Tỷ lệ gán nhãn phân loại đúng phòng ban đạt >= 92%.       │
│ - Tăng chỉ số hài lòng cư dân (CSAT) từ 3.8 ──> >= 4.6/5.   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Quick Problem Card #3 — Vinmec: Soạn thảo dự thảo Tóm tắt hồ sơ xuất viện

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tự động trích xuất thông tin lâm sàng từ  │
│ bệnh án điện tử (EMR) để soạn thảo dự thảo Tóm tắt hồ sơ     │
│ xuất viện (Discharge Summary) chuẩn y khoa cho bệnh nhân.   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị (quá tải văn bản hành  │
│ chính cuối ca), Bệnh nhân (chờ 2-3 tiếng để làm thủ tục).   │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Bác sĩ mở hồ sơ EMR của bệnh nhân sắp xuất viện        │
│   ──> 2. Đọc lại lịch sử xét nghiệm, diễn tiến điều trị     │
│   ──> 3. Gõ tay bản tóm tắt quá trình, kết quả & dặn dò     │
│   ──> 4. Bác sĩ kiểm tra lại thông tin và ký xác nhận       │
│   ──> 5. In hồ sơ và hướng dẫn chăm sóc cho người nhà       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 25-30 phút/   │
│ bệnh nhân; bác sĩ mệt mỏi dễ gõ tắt, sót chi tiết quan trọng)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (LLM tổng hợp dữ liệu xét nghiệm/thuốc từ EMR, draft sẵn    │
│ bản tóm tắt y khoa + bản dặn dò giải thích dễ hiểu).        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ - Rút ngắn thời gian soạn hồ sơ từ 30 phút ──> dưới 5 phút. │
│ - Bệnh nhân nhận hồ sơ xuất viện trong vòng 30 phút (cũ 3h). │
│ - Bắt buộc 100% hồ sơ phải được bác sĩ ký duyệt (HITL).      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### 🗳️ Đề xuất lựa chọn bài toán cho Phase 3 Deep-Dive:
* **Bài toán được chọn:** **Card #1 — Xanh SM: Xử lý sự cố pin và điều phối trạm sạc thực địa**.
* **Lý do lựa chọn:**
  1. **Tác động kinh doanh tức thì (Real-time ROI):** Trực tiếp giải quyết bài toán thời gian thực cho đội xe taxi điện đang vận hành trên đường, giảm thời gian chết của xe và tăng doanh thu cuốc xe.
  2. **Ranh giới an toàn rõ ràng (Strict Operational Boundary):** Có ranh giới rõ ràng về mặt vật lý và quy trình (ngưỡng pin 5%, khoảng cách trạm sạc 5km, bắt buộc gắn thẻ `[DRAFT_ONLY]` và yêu cầu điều phối viên bấm duyệt).
  3. **Phù hợp công nghệ (AI Fit):** Rất phù hợp với mô hình LLM Feature kết hợp Tool calling/Structured Output, không cần hệ thống Agentic phức tạp mà vẫn đạt hiệu quả vượt trội.
  4. **Lý do hoãn Card #2 & Card #3:**
     - *Card #2 (Vinhomes):* Rủi ro pháp lý và tranh chấp căn hộ cao, cần chuẩn hóa bộ quy tắc định tuyến (Rule-based) và phân loại dữ liệu khiếu nại trước khi áp dụng LLM diện rộng.
     - *Card #3 (Vinmec):* Lĩnh vực y tế đòi hỏi tuân thủ nghiêm ngặt chuẩn HIPAA/bảo mật hồ sơ sức khỏe và quy trình kiểm định lâm sàng phức tạp, chu kỳ triển khai dài hơn.

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Ai đang thực hiện tác vụ hằng ngày? |
| **2. Current Workflow** | Mô tả tóm tắt quy trình thủ công hiện tại và công cụ sử dụng. |
| **3. Bottleneck** | Bước nào chậm, lỗi, hoặc cần xử lý ngôn ngữ tự động nhiều nhất? |
| **4. Business Impact** | Tổn thất thực tế đo bằng thời gian, chi phí, hoặc SLA của Vingroup. |
| **5. Success Metric** | AI giải quyết được thì đạt ngưỡng số mấy? (Ví dụ: *"85% vé được phân loại dưới 10s"*). |
| **6. Operational Boundary** | AI được phép làm gì, TUYỆT ĐỐI không được làm gì, điểm nào cần duyệt? |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [ ] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
