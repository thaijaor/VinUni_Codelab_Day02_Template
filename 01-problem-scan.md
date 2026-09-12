# 01 — Problem Scan & Quick Cards (Vin Smart Future)

> **Họ và tên học viên:** Đỗ Tuấn Long
> **Đơn vị:** Vin Smart Future (Vingroup)
> **Mục tiêu:** Quét tìm các cơ hội tối ưu hóa bằng AI (Phase 1 — SCAN) và đánh giá nhanh qua 3 Quick Problem Cards (Phase 2 — QUICK-ASSESS).

---

# 🔍 Phase 1 — SCAN: Quét tìm bài toán bằng 4 Lenses

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên tiếp nhận và xử lý thủ công các báo cáo khẩn cấp từ tài xế taxi điện về sự cố cạn pin/trạm sạc quá tải trên đường (mất 12-15 phút/lượt tra cứu định vị GPS và tìm trụ sạc trống). |
| 2 | **Vinhomes** | Lặp lại | Phân loại tự động các phản ánh kỹ thuật (sự cố điện, rò rỉ nước sinh hoạt, hỏng thang máy) của cư dân trên App Vinhomes Resident để định tuyến tới đúng ban quản lý phân khu (mất 2-4 tiếng xử lý thủ công). |
| 3 | **Vinmec** | Tốn thời gian | Bác sĩ điều trị mất 25-30 phút/bệnh nhân để tổng hợp lịch sử xét nghiệm, diễn tiến điều trị từ hệ thống bệnh án điện tử (EMR) và soạn thảo bản tóm tắt hồ sơ xuất viện (Discharge Summary). |
| 4 | **VinFast** | AI-upgrade | Tiếp nhận phản ánh mô tả tiếng Việt tự nhiên của chủ xe (ví dụ: *"xe phát ra tiếng rít phanh khi rẽ trái ở tốc độ thấp"*) để tự động nhận diện và phân loại nhóm lỗi kỹ thuật sơ bộ, gợi ý đặt lịch xưởng dịch vụ 3S. |
| 5 | **Vinpearl / VinWonders** | Pain từ người khác | Nhân viên CSKH quá tải vào mùa cao điểm vì phải trả lời hàng nghìn câu hỏi lặp lại từ du khách về lịch biểu diễn, quy định mang hành lý/đồ ăn, chiều cao an toàn trò chơi và kiểm tra tình trạng vé vào cổng. |
| 6 | **VinFast** | Lặp lại | So khớp và đối soát hóa đơn sạc điện định kỳ giữa nhật ký phiên sạc (charging session logs) tại trạm sạc VinFast với báo cáo doanh thu tài chính để phát hiện sai lệch doanh thu. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 bài toán từ danh sách trên để tiến hành đánh giá nhanh:

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

---

### 🗳️ Đề xuất lựa chọn bài toán cho Phase 3 Deep-Dive:
* **Bài toán được chọn:** **Card #1 — Xanh SM: Xử lý sự cố pin và điều phối trạm sạc thực địa**.
* **Lý do lựa chọn:**
  1. **Tác động kinh doanh tức thì (Real-time ROI):** Trực tiếp giải quyết bài toán thời gian thực cho đội xe taxi điện đang vận hành trên đường, giảm thời gian chết của xe và tăng doanh thu cuốc xe.
  2. **Ranh giới an toàn rõ ràng (Strict Operational Boundary):** Có ranh giới rõ ràng về mặt vật lý và quy trình (ngưỡng pin 5%, khoảng cách trạm sạc 5km, bắt buộc gắn thẻ `[DRAFT_ONLY]` và yêu cầu điều phối viên bấm duyệt).
  3. **Phù hợp công nghệ (AI Fit):** Rất phù hợp với mô hình LLM Feature kết hợp Tool calling/Structured Output, không cần hệ thống Agentic phức tạp mà vẫn đạt hiệu quả vượt trội.
  4. **Lý do hoãn Card #2 & Card #3:**
     - *Card #2 (Vinhomes):* Rủi ro pháp lý và tranh chấp căn hộ cao, cần chuẩn hóa bộ quy tắc định tuyến (Rule-based) và phân loại dữ liệu khiếu nại trước khi áp dụng LLM diện rộng.
     - *Card #3 (Vinmec):* Lĩnh vực y tế đòi hỏi tuân thủ nghiêm ngặt chuẩn HIPAA/bảo mật hồ sơ sức khỏe và quy trình kiểm định lâm sàng phức tạp, chu kỳ triển khai dài hơn.
