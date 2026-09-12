# 02 — Báo Cáo Phân Tích Sâu (Deep-Dive Report)
## Dự án: Trợ lý Điều vận Thông minh Xanh SM — Điều phối Cứu hộ Sạc Pin Thực địa

> **Đơn vị thực hiện:** Nhóm AI Product Engineering — Vin Smart Future  
> **Khách hàng nội bộ:** Khối Vận hành GSM (Xanh SM) & Hệ thống Trạm sạc VinFast  
> **Tác giả nhánh đóng góp:** Hải (Branch: `hai-02482`)

---

# 🏗️ 1. Hiện Trạng Vận Hành (Current-State Workflow Mapping)

*(Sơ đồ quy trình trực quan chi tiết xem tại tệp: `04-workflow-diagram.png`)*

### Mô tả quy trình thủ công hiện tại:
* **Bước 1 (Tiếp nhận thông tin):** Tài xế xe taxi điện Xanh SM phát hiện pin cạn kiệt (dưới 5%) khi đang lưu thông. Tài xế tấp vào lề đường và gọi điện về Hotline Trung tâm Điều vận Xanh SM, đọc biển số xe và địa chỉ ước lượng bằng mắt thường. *(Thời gian: 2 - 3 phút)*.
* **Bước 2 (Tra cứu định vị GIS):** 🔄 **Handoff 1** — Điều phối viên tiếp nhận cuộc gọi, mở phần mềm giám sát hành trình nội bộ, gõ biển số xe để tìm vị trí toạ độ GPS thực tế của xe. *(Thời gian: 2 phút)*.
* **Bước 3 (Tra cứu trạm sạc & Tình trạng trụ):** 🔴 **Bottleneck 1** — Điều phối viên mở tiếp hệ thống quản lý trạm sạc VinFast (V-GREEN), rà soát thủ công các trạm sạc trong bán kính 3 - 5km. Cần kiểm tra xem trạm còn trụ sạc nhanh (DC 30kW/60kW) trống hay đang bị nghẽn/xếp hàng. *(Thời gian: 4 - 6 phút)*.
* **Bước 4 (Ra quyết định & Soạn tin nhắn hướng dẫn):** 🔴 **Bottleneck 2** — Điều phối viên cân nhắc: Nếu trạm quá xa (> 5km) hoặc hết trụ trống, xe có nguy cơ sập nguồn giữa đường. Điều phối viên phải gõ tin nhắn SMS/In-app thủ công gửi hướng dẫn cho tài xế hoặc gọi sang đội cứu hộ. *(Thời gian: 3 - 4 phút)*.
* **Bước 5 (Điều động xe sạc di động & Kết thúc sự cố):** 🔄 **Handoff 2** — Nếu pin cạn kiệt không thể tự di chuyển, điều phối viên gọi điện thoại bàn giao cho Đội Xe Sạc Di Động (Mobile Charging Van) xuất kích đến tọa độ GPS cứu hộ. *(Thời gian: 3 - 5 phút)*.

👉 **Tổng thời gian xử lý sự cố trung bình:** **14 — 20 phút / sự cố**.  
👉 **Điểm yếu chí mạng:** Vào khung giờ cao điểm, hàng chục tài xế gọi đồng thời khiến điều phối viên quá tải; việc tra cứu thủ công chậm trễ dẫn đến nhiều trường hợp xe hết sạch pin (0%) chết máy trên cầu hoặc giữa đường cao tốc, gây tắc đường và tổn hại nghiêm trọng đến thương hiệu Xanh SM / VinFast.

---

# 🎯 2. Bản Tuyên Bố Bài Toán 6 Trường (6-Field Problem Statement)

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | **Điều phối viên (Dispatcher)** tại Trung tâm Vận hành Xanh SM kết hợp cùng **Tài xế thực địa**. |
| **2. Current Workflow** | Tiếp nhận điện thoại báo cạn pin ──> Tra cứu vị trí xe trên bản đồ GIS ──> Tra cứu thủ công trạm sạc VinFast trống ──> Soạn tin nhắn hướng dẫn chỉ đường hoặc gọi đội cứu hộ lưu động. |
| **3. Bottleneck** | Khâu tra cứu chéo giữa 2 hệ thống (GPS xe & Tình trạng trụ sạc trống) mất 5-8 phút, và khâu soạn thảo tin nhắn chỉ dẫn/gọi đội cứu hộ hoàn toàn thủ công. |
| **4. Business Impact** | **Thiệt hại tài chính & SLA:** Mỗi ngày có trung bình 45 vụ sự cố cạn pin tại Hà Nội và TP.HCM. Chi phí kéo xe cứu hộ khi xe chết máy hoàn toàn là ~800.000 VNĐ/lượt. Tài xế mất từ 1.5 - 2 giờ doanh thu, tỉ lệ hủy chuyến giờ cao điểm tăng 4.2%, ảnh hưởng trực tiếp đến uy tín của taxi điện Xanh SM. |
| **5. Success Metric** | **Chỉ số định lượng rõ ràng:**<br>• Thời gian trung bình từ lúc nhận tin đến khi phát lệnh cứu hộ/chỉ đường giảm từ **15 phút xuống dưới 2 phút** (giảm 85%).<br>• 100% trường hợp pin `< 5%` kích hoạt lệnh điều xe sạc di động kịp thời, không có trường hợp xe chết máy giữa lòng đường.<br>• Tỉ lệ điều phối viên duyệt bản nháp AI hỗ trợ (Draft Acceptance Rate) đạt **>= 90%**. |
| **6. Operational Boundary (Ranh giới an toàn)** | **QUY TẮC CỐT LÕI:**<br>1. **Thẻ duyệt bắt buộc:** Mọi nội dung hướng dẫn gửi đến tài xế PHẢI luôn có thẻ `[DRAFT_ONLY]` ở đầu để điều phối viên con người kiểm tra trước khi bấm gửi.<br>2. **Ranh giới an toàn pin:** Khi pin xe `< 5%`, AI TUYỆT ĐỐI KHÔNG được gợi ý trạm sạc xa > 5km. Bắt buộc kích hoạt lệnh JSON `dispatch_mobile_charger`.<br>3. **Human-in-the-loop:** AI chỉ đóng vai trò Co-pilot đề xuất, không tự động trừ tiền ví của tài xế hay tự động gửi lệnh dispatch mà không có con người xác nhận. |

---

# 🤖 3. Luồng Tương Lai & Đánh Giá Mức Độ Phù Hợp Của AI (Future-State Flow & AI Fit)

### 3.1. Ma trận phân tích AI Fit (AI-Fit Matrix)

| Phương pháp kỹ thuật | Đánh giá tính khả thi | Kết luận |
|---|---|---|
| **Thuần Rule-based / CSDL** | Chỉ tra cứu được tọa độ số học cứng, không hiểu được tin nhắn báo sự cố phức tạp bằng tiếng Việt từ tài xế (ví dụ: *"xe kẹt ở chân cầu Vĩnh Tuy, pin còn 2% đang chở người bệnh"*). | Không đủ linh hoạt |
| **LLM Feature (Co-pilot)** | ✅ Phù hợp nhất. LLM trích xuất ngữ cảnh, kiểm tra ngưỡng pin, sinh cấu trúc lệnh JSON điều xe sạc, đồng thời soạn thảo tin nhắn hướng dẫn chuẩn mực có thẻ `[DRAFT_ONLY]`. | **LỰA CHỌN TỐI ƯU** |
| **Autonomous Multi-Agent Loop** | Rủi ro quá cao trong giai đoạn hiện tại. Cho phép Agent tự động gọi xe cứu hộ mà không qua con người dễ gây dispatch sai vị trí, tốn chi phí vận hành hàng trăm triệu đồng. | Không khả thi (Over-engineering) |

### 3.2. Sơ đồ luồng tương lai (Future-State Flow):

```text
[Tài xế gửi tin nhắn/cuộc gọi sự cố]
                │
                ▼
      ┌──────────────────┐
      │  Hệ thống Xanh SM│ (Trích xuất Telemetry: Tọa độ GPS, % Pin hiện tại)
      └─────────┬────────┘
                │
                ▼
   🔵 [AI Step: Gemini 2.5 Flash Co-pilot]
      - Phân tích mức pin và toạ độ.
      - Nếu Pin < 5%: Tạo lệnh JSON {"action": "dispatch_mobile_charger"}
      - Tạo bản nháp tin nhắn hướng dẫn kèm tiền tố [DRAFT_ONLY].
                │
                ▼
   🟢 [Human Step: HITL - Điều phối viên duyệt]
      - Điều phối viên xem màn hình Co-pilot (mất 5-10 giây).
      - Bấm [Phê duyệt / Gửi lệnh] hoặc chỉnh sửa nhanh nếu có phát sinh thực địa.
                │
          ┌─────┴─────┐
   (Duyệt thành công) (LLM Timeout / Lỗi mạng)
          │           │
          ▼           ▼
[Gửi lệnh cứu hộ]  ↩️ [Fallback Step]
                   Hệ thống tự động chuyển sang quy trình Rule-based:
                   Hiển thị danh sách 3 số điện thoại đội cứu hộ khu vực gần nhất
                   để điều phối viên bấm gọi trực tiếp bằng đường truyền dây thoại.
```

---

# 🏁 4. Đánh Giá Sẵn Sàng & Quyết Định (AI Readiness & Evaluation)

### 4.1. Bảng kiểm tra độ sẵn sàng (AI Readiness Checklist):
* [x] **Dữ liệu & Telemetry:** Đã có hệ thống dữ liệu viễn thông xe (VinFast Telematics) và cơ sở dữ liệu vị trí trụ sạc V-GREEN sẵn có qua API nội bộ.
* [x] **Kiểm soát rủi ro an toàn:** Rủi ro sai sót được kiểm soát 100% thông qua cơ chế **Human-in-the-loop (HITL)** với thẻ ranh giới `[DRAFT_ONLY]` và kịch bản `Fallback` tức thì.
* [x] **Sự đón nhận của Stakeholders:** Điều phối viên rất hoan nghênh vì giảm được 80% gánh nặng thao tác thủ công trong giờ cao điểm.

### 4.2. Quyết định cuối cùng:
**[x] GO (Bắt đầu xây dựng Prototype phạm vi hẹp)**  
[ ] NOT YET (Chưa đủ điều kiện)  
[ ] NO-GO (Hủy bỏ dự án)

### 4.3. Lý giải quyết định (Justification):
1. **Lợi ích kinh tế vượt trội:** Giảm 85% thời gian xử lý mỗi sự cố (từ 15 phút còn 2 phút), tiết kiệm ước tính 360 triệu VNĐ/tháng chi phí xe cứu hộ kéo khẩn cấp và bảo vệ doanh thu giờ cao điểm cho đội ngũ tài xế.
2. **Kiến trúc kỹ thuật rõ ràng, chi phí thấp:** Không cần đào tạo mô hình riêng tốn kém; sử dụng Gemini 2.5 Flash với System Prompt nghiêm ngặt cho độ trễ phản hồi cực nhanh (< 1 giây) và chi phí API tính trên từng token cực kỳ thấp.
3. **An toàn tuyệt đối:** Ranh giới vận hành đã được kiểm chứng thông qua bộ test đối kháng (Adversarial Tests) tại `starter-code/prompt_prototype.py`, loại trừ nguy cơ gửi tin nhắn tự động khi chưa có sự xác nhận của con người.
