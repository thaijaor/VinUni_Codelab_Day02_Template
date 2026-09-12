# 📋 02 — Deep-Dive Report: AI Product Scoping (Vin Smart Future)

**Đơn vị:** Vin Smart Future (Vingroup)  
**Mảng nghiệp vụ:** GSM (Xanh SM) & VinFast Ecosystem  
**Tên dự án AI:** Hệ thống Trợ lý Điều phối Thông minh & Xử lý Sự cố Pin Xe Điện Thực địa (Xanh SM Smart Battery Dispatcher Co-pilot)  
**Nhóm tác giả:** AI Product Engineering Team  

---

## 🏛️ 1. Bối cảnh & Mục tiêu dự án

Trong chiến lược phát triển hệ thống giao thông thuần điện của Vingroup, **Xanh SM (GSM)** hiện đang vận hành hàng chục nghìn ô tô điện và xe máy điện trên khắp cả nước với tần suất hoạt động 24/7. 

Vào các khung giờ cao điểm hoặc điều kiện thời tiết khắc nghiệt (nắng nóng, ngập úng), tài xế gặp phải các sự cố khẩn cấp liên quan đến pin (dung lượng pin giảm nhanh, không tìm được trụ sạc trống, pin chạm ngưỡng nguy hiểm < 5%). Khi đó, việc xử lý thủ công qua tổng đài điều vận tạo ra nút thắt cổ chai lớn:
* Điều phối viên quá tải, thao tác chậm trên nhiều hệ thống riêng rẽ.
* Thời gian phản hồi kéo dài từ 15 - 20 phút.
* Nguy cơ xe cạn sạch pin (0%) giữa đường, gây ùn tắc giao thông, hư hại pin và tổn hại hình ảnh thương hiệu Vingroup.

Dự án này hướng đến việc tích hợp mô hình ngôn ngữ lớn (LLM Feature) và tự động hóa quy trình nghiệp vụ để hỗ trợ điều phối viên tra cứu, phân tích và soạn thảo phương án ứng cứu tức thì cho tài xế với độ an toàn tuyệt đối.

---

## 🏗️ 2. Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow Mapping (Quy trình vận hành hiện tại)

Quy trình xử lý sự cố hết pin thực địa hiện hành tại Trung tâm Điều vận Xanh SM bao gồm 5 bước tuần tự:

```text
┌────────────────┐      ┌────────────────┐      ┌────────────────┐      ┌────────────────┐
│ Bước 1         │      │ Bước 2         │      │ Bước 3         │      │ Bước 4         │
│ Tiếp nhận cuộc │      │ Tra cứu định   │      │ Tra cứu trạm   │      │ Soạn thảo tin  │
│ gọi sự cố      │ ───> │ vị GPS & pin   │ ───> │ sạc VinFast    │ ───> │ nhắn chỉ đường │
│                │      │                │      │ còn trụ trống  │      │ gửi tài xế     │
│ Actor: Dispatch│      │ Actor: Dispatch│      │ Actor: Dispatch│      │ Actor: Dispatch│
│ ⏱ 2 phút      │      │ ⏱ 2 phút      │      │ ⏱ 5 phút 🔴   │      │ ⏱ 5 phút 🔴   │
│ In: Cuộc gọi   │ 🔄   │ In: Biển số xe │ 🔄   │ In: Tọa độ GPS │ 🔄   │ In: Dữ liệu thô│
│ Out: Log sự cố │      │ Out: Tọa độ GPS│      │ Out: Địa chỉ   │      │ Out: SMS/App   │
└────────────────┘      └────────────────┘      └────────────────┘      └────────────────┘
                                                                                 │
                                                                                 ▼
                                                                        ┌────────────────┐
                                                                        │ Bước 5         │
                                                                        │ Gọi xe cứu hộ  │
                                                                        │ sạc lưu động   │
                                                                        │ (nếu cạn kiệt) │
                                                                        │ Actor: Dispatch│
                                                                        │ ⏱ 1 phút      │
                                                                        └────────────────┘

Ký hiệu:
🔴 Bottleneck: Nút thắt cổ chai gây tốn nhiều thời gian và dễ sai sót nhất.
🔄 Handoff: Điểm chuyển giao thông tin thủ công giữa người dùng và các dashboard phần mềm.
⏱ Tổng thời gian xử lý thủ công: 15 phút / lượt sự cố.
```

#### Phân tích điểm nghẽn & rủi ro:
1. **Bottleneck 1 (Bước 3 — ⏱ 5 phút):** Điều phối viên phải chuyển qua màn hình bản đồ VinFast Station Dashboard, lọc loại cổng sạc (CCS2 cho VF e34/VF5/VF8), kiểm tra số trụ trống thời gian thực và đo khoảng cách ước tính. Vào giờ cao điểm, trạm trống có thể bị xe khác cắm sạc ngay trong lúc đang tra cứu.
2. **Bottleneck 2 (Bước 4 — ⏱ 5 phút):** Điều phối viên phải gõ tay tin nhắn chỉ đường bằng tiếng Việt có dấu, mô tả chi tiết lối vào hầm/bãi đỗ trung tâm thương mại Vincom. Do áp lực thời gian, tin nhắn hay bị sai lỗi chính tả hoặc thiếu thông tin trụ sạc phù hợp.
3. **Rủi ro chí mạng:** Nếu xe còn dưới 5% pin mà điều phối viên chỉ dẫn đến trạm cách xa trên 5km, xe sẽ dừng máy giữa đại lộ, gây tai nạn hoặc phải kéo xe tốn kém.

---

### 3.2. Problem Statement (6-field) — Tiêu chuẩn Vin Smart Future

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Vận hành & Điều vận Xanh SM (GSM Hà Nội & TP.HCM). |
| **2. Current Workflow** | Khi tài xế gọi báo sự cố pin, điều phối viên nhập biển số tra GPS trên hệ thống giám sát hành trình GSM, mở bản đồ trạm sạc VinFast kiểm tra trụ trống, nhẩm tính cự ly, gõ tin nhắn SMS chỉ đường gửi tài xế, hoặc chuyển máy sang đội xe cứu hộ VinFast nếu xe đã cạn kiệt pin. Toàn bộ qua 5 bước thủ công, mất trung bình 15 phút/lượt. |
| **3. Bottleneck** | Bước 3 & 4 (chiếm 10/15 phút): Tra cứu thủ công trụ sạc trống tương thích dòng xe và gõ văn bản hướng dẫn chỉ đường chi tiết cho tài xế. |
| **4. Business Impact** | Toàn hệ thống ghi nhận trung bình ~120 sự cố pin/ngày tại các thành phố lớn. Lãng phí 30 giờ công lao động/ngày của đội ngũ tổng đài. Tăng thời gian chết của xe (idle vehicle time), giảm doanh thu ~15-20% trong ca làm việc của tài xế và gây bức xúc cho hành khách đang đợi xe. |
| **5. Success Metric** | **Hiệu suất vận hành (Efficiency):** Giảm tổng thời gian xử lý sự cố từ 15 phút xuống dưới **3 phút/lượt** (giảm 80% thời gian xử lý).<br>**Độ chính xác (Quality):** Tỉ lệ chỉ dẫn chính xác trạm sạc khả dụng và tương thích đạt **≥ 98%**.<br>**An toàn pin (Zero Critical Breakdown):** 100% trường hợp pin < 5% được nhận diện và tự động chuyển sang điều xe cứu hộ sạc di động (Zero false station routing under critical battery). |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Tự động tổng hợp dữ liệu GPS, dữ liệu trạm sạc VinFast; tự động phân tích mức pin và đề xuất phương án; tự động soạn thảo tin nhắn hướng dẫn dạng bản nháp (DRAFT).<br>**AI TUYỆT ĐỐI CẤM:** CẤM tự động phát lệnh gửi tin nhắn ra ngoài tới tài xế mà chưa qua nút bấm duyệt của điều phối viên (Bắt buộc Human-in-the-loop); CẤM đề xuất trạm sạc xa > 5km cho xe có dung lượng pin < 5%; CẤM tự ý hứa hẹn bồi thường cuốc xe cho tài xế. |

---

### 3.3. Future-State Flow & AI Fit (Quy trình tương lai & Mức độ phù hợp AI)

#### 📊 So sánh AI-Fit Matrix:

| Tiêu chí | Option 1: Rule-Based Logic | Option 2: LLM Feature (ĐƯỢC CHỌN) | Option 3: Autonomous Multi-Agent |
|---|---|---|---|
| **Khả năng xử lý ngôn ngữ** | Rất kém, chỉ gửi template cứng nhắc, không cá nhân hóa theo tình huống xe/khách. | Rất tốt: Diễn đạt tự nhiên, linh hoạt kết hợp địa chỉ, lối vào hầm, cảnh báo an toàn. | Dư thừa năng lực, khó kiểm soát độ trễ và chi phí token. |
| **Tính xác định & An toàn** | Cao, nhưng cứng nhắc và khó mở rộng khi có nhiều biến số mới. | Cao khi có System Instructions và Output Guardrails (`[DRAFT_ONLY]`, ngưỡng pin). | Kém: Agentic loop có nguy cơ lặp vô tận, tự ý ra quyết định ngoài tầm kiểm soát. |
| **Thời gian triển khai & Chi phí** | Thấp, nhưng trải nghiệm người dùng kém. | Tối ưu: Tận dụng Gemini Flash tốc độ cao (< 1.5s), chi phí token cực thấp. | Rất cao, bảo trì phức tạp, rủi ro ảo giác (hallucination) cao. |
| **Kết luận AI Fit** | Loại (không giải quyết triệt để Bottleneck 4). | **LỰA CHỌN TỐI ƯU:** Kết hợp Rule check cứng + LLM soạn thảo thông minh + Human duyệt. | Loại (quá phức tạp, rủi ro an toàn vận hành cao). |

#### 🔄 Future-State Workflow (Sơ đồ quy trình tương lai):

```text
┌──────────────────┐       ┌────────────────────────┐       ┌────────────────────────┐       ┌────────────────────────┐
│ Bước 1           │       │ Bước 2                 │       │ Bước 3                 │       │ Bước 4                 │
│ Tiếp nhận cuộc   │       │ 🔵 AI Integration      │       │ 🔵 AI Drafting &       │       │ 🟢 Human-in-the-loop   │
│ gọi sự cố        │ ────> │ Auto-pull GPS xe &     │ ────> │ Phân loại tình huống   │ ────> │ Dispatcher click duyệt │
│                  │       │ Tra cứu VinFast API    │       │ Sinh tin nhắn nháp     │       │ hoặc chỉnh sửa & gửi   │
│ Actor: Dispatch  │       │ Hệ thống tự động       │       │ Gemini Flash Engine    │       │ Actor: Dispatcher      │
│ ⏱ 30 giây        │       │ ⏱ 3 giây               │       │ ⏱ 2 giây               │       │ ⏱ 30 giây              │
└──────────────────┘       └────────────────────────┘       └────────────────────────┘       └────────────────────────┘
                                                                                                         │
                                                                           ┌─────────────────────────────┴─────────────────────────────┐
                                                                           ▼                                                           ▼
                                                            [Trường hợp pin < 5%]:                                      ↩️ Fallback Plan:
                                                            AI tự động nhận diện & kích hoạt                            Nếu LLM timeout (>5s) hoặc
                                                            lệnh cứu hộ sạc di động:                                    bị lỗi API, hệ thống tự động
                                                            `{"action": "dispatch_mobile_charger"}`                     chuyển về giao diện tra cứu
                                                            Dispatcher nhấn "Xác nhận cứu hộ"                           bản đồ thủ công như cũ.
```

---

## 💻 4. Phase 4 — Kỹ thuật Prompt Prototype & Ranh giới an toàn

Nhóm đã hiện thực hóa giải pháp thành mã nguồn Python có thể chạy tự động tại [`starter-code/prompt_prototype.py`](starter-code/prompt_prototype.py) trên nền tảng **Google Gemini Flash** (`gemini-3.6-flash`).

### 2 Ranh giới an toàn then chốt (Guardrails):
1. **Human Review Enforcer:** Mô hình bắt buộc phải xuất phát mọi phản hồi bằng tiền tố `[DRAFT_ONLY]`. Nếu không có thẻ này, hệ thống giao tiếp phía client sẽ chặn không cho hiển thị lên giao diện gửi tin.
2. **Critical Battery Interceptor:** Mô hình được lập trình để phát hiện mức pin dưới 5% (`battery < 5%`). Khi phát hiện, mô hình bị cấm tuyệt đối việc gợi ý trạm sạc cách xa trên 5km, và bắt buộc phải trả về payload JSON điều xe cứu hộ: `{"action": "dispatch_mobile_charger", "reason": "..."}`.

### Kết quả Adversarial Testing:
* Cả 3 kịch bản tấn công (ép gửi trạm xa khi pin 2%, yêu cầu bỏ thẻ draft, và prompt injection override) đều bị hệ thống chặn thành công 100%. Ranh giới vận hành giữ vững tuyệt đối.

---

## 🏁 5. Phase 5 — EVALUATE: Đánh giá độ sẵn sàng & Quyết định

### 5.1. AI Readiness Checklist

| Tiêu chí kiểm tra độ sẵn sàng | Trạng thái | Đánh giá thực tế tại Vin Smart Future |
|---|:---:|---|
| **1. Dữ liệu mẫu & Telemetry sạch:** | ✅ ĐẠT | Hệ thống GSM và VinFast đã có sẵn API Realtime telemetry (GPS xe, % SoC pin, trạng thái từng trụ sạc VinFast đang rảnh/bận). |
| **2. Kiểm soát rủi ro (Risk Containment):** | ✅ ĐẠT | Có chốt chặn kép: Thẻ `[DRAFT_ONLY]` bắt buộc điều phối viên duyệt (HITL) + Kế hoạch dự phòng (Fallback) tự động fallback về thủ công nếu AI gián đoạn. |
| **3. Sự sẵn sàng của Stakeholders:** | ✅ ĐẠT | Khối Vận hành GSM rất ủng hộ vì giải pháp giúp giảm áp lực trực tiếp cho nhân viên điều vận trong giờ cao điểm mà không làm thay đổi vai trò quyết định của họ. |

### 5.2. Quyết định của Ban Giám Đốc Vin Smart Future:

**LỰA CHỌN: [x] GO (Bắt đầu xây dựng Prototype & Thử nghiệm thực địa)**

#### 📝 Justification (Lý giải quyết định):
1. **Giá trị kinh tế và vận hành rõ rệt:** Giảm 80% thời gian xử lý sự cố (từ 15 phút xuống dưới 3 phút). Tiết kiệm hơn 900 giờ lao động/tháng cho trung tâm điều vận, đồng thời giảm thiểu tối đa tổn thất do xe điện nằm chết trên đường.
2. **Chi phí kỹ thuật cực thấp:** Sử dụng kiến trúc **LLM Feature** với Gemini Flash (chi phí token trung bình chỉ ~$0.0001 / lượt xử lý sự cố), không cần đầu tư hạ tầng GPU tự host đắt đỏ.
3. **Mức độ rủi ro gần như bằng 0 (Zero-Risk Architecture):** Nhờ cơ chế Human-in-the-loop, AI không có quyền tự quyết gửi tin ra bên ngoài. Nếu AI trả về kết quả không hoàn hảo, điều phối viên có thể chỉnh sửa trong vài giây hoặc sử dụng quy trình thủ công dự phòng.

Dự án đạt chuẩn triển khai Sprint 1 (2 tuần) để tích hợp thử nghiệm trên 50 xe taxi điện Xanh SM tại khu vực Hà Nội.
