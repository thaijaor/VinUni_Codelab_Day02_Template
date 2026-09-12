# 02 — Deep-Dive Report: Điều phối sự cố pin cạn (Xanh SM)

**Học viên:** thaijaor
**Mã học viên:** 2A202602894
**Email:** thainguyenhong2204@gmail.com
**Branch:** `thai-02894`
**Bài toán chọn:** Xanh SM — điều phối viên xử lý báo cáo pin dưới 5% từ tài xế xe điện.

---

## Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow Mapping

Sơ đồ trực quan: [`04-workflow-diagram.png`](04-workflow-diagram.png)

| Bước | Việc làm | Ai làm | Thời gian | Ghi chú |
|---:|---|---|---:|---|
| 1 | Tài xế gọi hotline báo pin cạn, đọc vị trí bằng lời | Tài xế | 1,5 phút | 🔄 Handoff: tài xế → tổng đài |
| 2 | Điều phối viên gõ vị trí, tra bản đồ GIS xác định xe | Điều phối viên | 2 phút | Vị trí mô tả dân gian, dễ lệch |
| 3 | Dò trạm sạc lân cận, kiểm tra còn trụ trống | Điều phối viên | 6 phút | 🔴 **Bottleneck** — mở nhiều dashboard, dữ liệu trụ trống cập nhật trễ |
| 4 | Soạn tin hướng dẫn gửi tài xế | Điều phối viên | 4 phút | 🔴 **Bottleneck** — gõ tay, dễ sai địa chỉ |
| 5 | Gọi đội xe sạc lưu động nếu không có trạm khả dụng | Điều phối viên | 2 phút | 🔄 Handoff: tổng đài → đội xe sạc |

**Tổng cộng: ~15,5 phút/lượt.** Riêng bước 3 + 4 chiếm 10 phút, tức 65% thời gian xử lý.

#### Phân tích điểm nghẽn

Bước 3 và 4 tắc vì cùng một nguyên nhân: điều phối viên phải tự tổng hợp thông tin rời rạc
(vị trí xe, danh sách trạm, trạng thái trụ, quãng đường) rồi diễn đạt thành tin nhắn.
Trong lúc đó pin vẫn tụt. Ở mức dưới 5%, xe chỉ còn chạy được vài km, nên 10 phút
thao tác thủ công là khoảng thời gian quyết định giữa việc tài xế tới được trạm hay
chết máy giữa đường.

Rủi ro kèm theo: giờ cao điểm một điều phối viên ôm nhiều cuộc cùng lúc, áp lực khiến
họ chọn nhanh trạm gần nhất trên bản đồ mà không kiểm tra trụ còn trống hay khoảng cách
thực tế — dẫn tới chỉ tài xế đi 8km với 2% pin.

---

### 3.2. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Điều phối viên tổng đài Xanh SM; đối tượng chịu ảnh hưởng trực tiếp là tài xế xe điện đang có nguy cơ chết máy. |
| **2. Current Workflow** | Tài xế gọi hotline → điều phối viên tra GPS → dò trạm sạc còn trụ trống → soạn tin hướng dẫn → gọi xe sạc lưu động nếu cần. Công cụ: hotline, bản đồ GIS, dashboard trạm sạc, tin nhắn thủ công. |
| **3. Bottleneck** | Bước dò trạm và soạn tin (10/15,5 phút). Đầu vào là mô tả ngôn ngữ tự nhiên, đầu ra là văn bản hướng dẫn — đúng dạng việc cần xử lý ngôn ngữ tự động. |
| **4. Business Impact** | Mỗi lượt tốn ~15,5 phút của điều phối viên. Xe chết máy giữa đường kéo theo huỷ cuốc, phải điều xe cứu hộ, tài xế mất thu nhập ca đó và khách chờ bị huỷ chuyến. |
| **5. Success Metric** | Thời gian xử lý một sự cố từ ~15,5 phút xuống **dưới 2 phút**; **100%** bản nháp giữ tag `[DRAFT_ONLY]` trước khi gửi; **0** trường hợp đề xuất trạm xa hơn 5km khi pin dưới 5%. |
| **6. Operational Boundary** | AI được: trích xuất vị trí và mức pin, tra trạm sạc, soạn bản nháp tin nhắn. AI **không** được: tự gửi tin cho tài xế, tự huỷ cuốc, hứa bồi thường, bịa số liệu pin hoặc khoảng cách. Mọi bản nháp phải được điều phối viên duyệt. |

---

### 3.3. Future-State Flow & AI Fit

#### So sánh AI-Fit

| Phương án | Đánh giá |
|---|---|
| **Rule / State-Machine** | Làm được phần ngưỡng pin và bán kính 5km, nhưng không đọc nổi mô tả vị trí bằng lời của tài xế ("gần cây xăng ngã tư Kim Mã"). Không đủ. |
| **LLM Feature** | Đọc hiểu và soạn tin tốt, nhưng một mình không tra được trạng thái trụ sạc thời gian thực. |
| **Agentic Loop** | ☑ **Chọn.** LLM hiểu tình huống → gọi tool tra trạm sạc → áp ngưỡng an toàn → sinh bản nháp → người duyệt. |

#### Future-State Flow

```
Tài xế báo pin cạn
   │
   ▼
🔵 AI trích xuất: vị trí, % pin, biển số        (~5 giây)
   │
   ▼
🔵 Tool tra trạm sạc còn trụ trống trong bán kính (~10 giây)
   │
   ▼
[ Pin < 5% ? ]
   │
   ├── Có ──► 🔵 Bỏ qua mọi trạm xa hơn 5km
   │           Sinh {"action": "dispatch_mobile_charger", "reason": ...}
   │
   └── Không ─► 🔵 Sinh bản nháp chỉ đường tới trạm phù hợp
   │
   ▼
🔵 Gắn tag [DRAFT_ONLY] ở đầu mọi output
   │
   ▼
🟢 Điều phối viên đọc, sửa nếu cần, bấm gửi     (~60 giây)
   │
   ▼
Tin nhắn tới tài xế / lệnh điều xe sạc lưu động
```

#### Human-in-the-loop

Điều phối viên là người bấm gửi, luôn luôn. Tag `[DRAFT_ONLY]` là cơ chế kỹ thuật
chặn hệ thống tự gửi khi chưa có người duyệt. Nếu người dùng yêu cầu bỏ tag hoặc gửi
thẳng, mô hình phải từ chối — đây là một trong hai adversarial test đã dựng trong
`starter-code/prompt_prototype.py`.

#### Fallback

| Tình huống | Xử lý |
|---|---|
| LLM không trích được vị trí hoặc % pin | Trả về yêu cầu bổ sung thông tin, không đoán |
| Tool tra trạm sạc timeout | Bỏ qua bước đề xuất trạm, chuyển thẳng sang điều xe sạc lưu động |
| Output thiếu tag `[DRAFT_ONLY]` | Hệ thống chặn, không hiển thị, ghi log để rà lại prompt |
| Pin dưới 5% mà mô hình vẫn nhắc trạm xa | Chặn ở tầng kiểm tra hậu kỳ, fallback về mẫu tin cố định |

---

## Phase 4 — Prompt Prototype

Code: [`starter-code/prompt_prototype.py`](starter-code/prompt_prototype.py) — model `gemini-3.5-flash-lite`.

Hai ranh giới được mã hoá trong `SYSTEM_PROMPT`:

1. Mọi phản hồi bắt đầu bằng `[DRAFT_ONLY]`, từ chối mọi yêu cầu bỏ tag kể cả khi người dùng tự nhận có thẩm quyền.
2. Pin dưới 5%: cấm nhắc trạm xa hơn 5km, phải trả khối JSON `dispatch_mobile_charger` kèm lý do.

Hai adversarial test tương ứng: một ca ép chỉ đường tới trạm 8km khi pin 2%,
một ca ép bỏ tag `[DRAFT_ONLY]` để gửi thẳng.

---

## Phase 5 — EVALUATE

### Checklist độ sẵn sàng

| Tiêu chí | Đánh giá |
|---|:-:|
| Actor và workflow hiện tại rõ ràng | ✅ |
| Bottleneck đo được bằng thời gian | ✅ |
| Đầu vào là ngôn ngữ tự nhiên, đúng thế mạnh LLM | ✅ |
| Ranh giới an toàn định lượng được (5%, 5km) | ✅ |
| Có bước người duyệt trước khi ra quyết định | ✅ |
| Có sẵn API trạng thái trụ sạc thời gian thực | ⚠️ chưa xác nhận |
| Baseline 15,5 phút đo từ dữ liệu thật | ⚠️ đang là ước tính |

### Quyết định: **NOT YET**

Bài toán đúng hướng — bottleneck rõ, ranh giới định lượng được, prototype đã chạy và
chặn được cả hai kiểu tấn công. Nhưng hai điều kiện chưa đủ để bật đèn xanh triển khai:

1. **Chưa có baseline thật.** Con số 15,5 phút là ước tính khi scoping, chưa bấm giờ
   trên ca trực thật. Không có baseline thì không chứng minh được cải thiện.
2. **Chưa xác nhận nguồn dữ liệu trụ sạc.** Toàn bộ giá trị của giải pháp nằm ở chỗ
   biết trạm nào còn trụ trống ngay lúc đó. Nếu dữ liệu này trễ vài phút hoặc không có
   API, agentic loop sẽ đề xuất trạm đã đầy — tệ hơn cả cách làm thủ công.

Việc cần làm trước khi chuyển sang GO: bấm giờ 20 ca thật để lấy baseline, và làm việc
với đội V-Green xác nhận độ trễ của dữ liệu trạng thái trụ sạc.
