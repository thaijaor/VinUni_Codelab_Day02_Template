# 🔍 01 — Problem Scan & Quick Problem Cards (Vin Smart Future)

**Đơn vị:** Vin Smart Future (Vingroup)  
**Nhóm tác giả:** AI Product Engineering Team  
**Mục tiêu:** Quét tìm các điểm nghẽn (bottlenecks) và cơ hội tối ưu hóa bằng AI trong toàn bộ hệ sinh thái các công ty thành viên Vingroup (Xanh SM, VinFast, Vinhomes, Vinmec, Vinpearl).

---

## 🧭 Phase 1 — SCAN: Quét cơ hội vận hành bằng 4 Lenses

Sử dụng **4 Lenses AI Scoping** (Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác) để nhận diện các quy trình thủ công đang làm rò rỉ hiệu suất tại Vingroup:

| # | Subsidiary | Tên quy trình / Bài toán | Lens | Mô tả ngắn bài toán & Điểm nghẽn thực tế |
|---|---|---|---|---|
| **1** | **Xanh SM (GSM)** | Điều phối xử lý sự cố sạc pin / hết pin thực địa | **Tốn thời gian** | Điều phối viên mất 15-20 phút/sự cố để tra cứu thủ công vị trí xe qua GPS, kiểm tra trạm sạc VinFast còn trụ trống trên dashboard, và soạn thảo tin nhắn chỉ đường gửi tài xế hoặc gọi cứu hộ. |
| **2** | **VinFast** | Đối chiếu hóa đơn & sản lượng điện trạm sạc đối tác | **Lặp lại** | Chuyên viên tài chính - vận hành phải so khớp thủ công hàng chục nghìn dòng giao dịch sạc điện hàng tuần giữa log telemetry của trụ sạc VinFast với bảng kê hóa đơn thanh toán của đối tác liên kết ngoài. |
| **3** | **Vinhomes** | Phân loại & điều hướng phản ánh cư dân (App Vinhomes Resident) | **AI có thể tốt hơn** | Cư dân gửi hàng nghìn phản ánh tự do (nước yếu, tiếng ồn, hỏng đèn, an ninh). CSKH phân loại thủ công mất 4-12 giờ, trả lời rập khuôn gây bức xúc, dễ chuyển nhầm ban quản lý tòa nhà. |
| **4** | **Vinmec** | Soạn thảo tóm tắt hồ sơ xuất viện (Clinical Discharge Summary) | **Pain từ người khác** | Bác sĩ lâm sàng quá tải hành chính, mất 25-30 phút/bệnh nhân để tổng hợp các kết quả xét nghiệm, chẩn đoán, toa thuốc vào bản tóm tắt xuất viện bằng ngôn ngữ dễ hiểu cho người bệnh. |
| **5** | **Xanh SM (GSM)** | Phân tích lý do hủy chuyến & rò rỉ cuốc xe giờ cao điểm | **Pain từ người khác** | Khách hủy chuyến do tài xế đón lâu hoặc định vị sai. Đội ngũ QA phải nghe lại thủ công mẫu ghi âm cuộc gọi và đọc note tài xế để thống kê pattern nguyên nhân, chậm trễ phản hồi cho sản phẩm. |
| **6** | **Vinpearl / VinWonders**| Tự động hóa trích xuất yêu cầu đặt phòng đoàn (Group Booking) | **Lặp lại & Tốn thời gian** | Đại lý lữ hành gửi email đặt phòng cho hàng trăm khách với yêu cầu phòng, bữa ăn, lịch vui chơi phức tạp dưới dạng bảng biểu phi cấu trúc. Nhân viên sales mất 45 phút/đoàn để nhập liệu vào hệ thống PMS. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn lọc **3 bài toán tiềm năng nhất** từ danh sách trên để phân tích sơ bộ tính khả thi:

### 🎴 Thẻ bài toán #1: Xanh SM — Xử lý sự cố hết pin thực địa của tài xế taxi điện

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                           │
│                                                                                 │
│ Bài toán (1 câu): Tài xế Xanh SM báo cáo xe cạn kiệt pin giữa đường cần điều   │
│ phối trạm sạc trống gần nhất hoặc xe cứu hộ sạc pin di động khẩn cấp.           │
│ Công ty thành viên: [x] Xanh SM (GSM)      [x] VinFast                          │
│                                                                                 │
│ Ai đang đau (Actor)? Tài xế Xanh SM (chờ đợi, mất doanh thu),                   │
│                      Điều phối viên Trung tâm Điều vận (quá tải giờ cao điểm)   │
│                                                                                 │
│ Workflow thủ công hiện tại (5 bước):                                            │
│   1. Nhận cuộc gọi khẩn ──> 2. Tra cứu GPS vị trí ──> 3. Tra trạm sạc trống     │
│   ──> 4. Soạn tin nhắn hướng dẫn ──> 5. Điều xe cứu hộ pin (nếu cạn kiệt)      │
│                                                                                 │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10-12 phút/lượt)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4 (Đọc tọa độ + pin, tự động     │
│ check trạm sạc phù hợp, draft tin chỉ dẫn hoặc kích hoạt cứu hộ).               │
│                                                                                 │
│ Đo thành công bằng gì (Metric có số)?                                           │
│   - Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt.                 │
│   - Độ chính xác trạm sạc & cổng sạc đúng loại xe đạt 98%.                      │
│                                                                                 │
│ Quick Architecture: [x] LLM Feature (với Human-in-the-loop & Rule Guardrails)   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

### 🎴 Thẻ bài toán #2: Vinhomes — Tự động phân loại & định tuyến phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                           │
│                                                                                 │
│ Bài toán (1 câu): Tự động phân loại nội dung phản ánh tự do của cư dân trên app │
│ Vinhomes Resident và điều hướng trực tiếp đến đúng bộ phận phụ trách của tòa nhà│
│ Công ty thành viên: [x] Vinhomes                                                │
│                                                                                 │
│ Ai đang đau (Actor)? Cư dân Vinhomes (chờ phản hồi lâu),                        │
│                      Nhân viên chăm sóc khách hàng BQL Tòa nhà                  │
│                                                                                 │
│ Workflow thủ công hiện tại (4 bước):                                            │
│   1. Đọc phản ánh ──> 2. Gán nhãn thủ công (Điện/Nước/Vệ sinh/An ninh)         │
│   ──> 3. Chuyển tiếp ticket cho kỹ thuật tòa nhà ──> 4. Soạn phản hồi cư dân    │
│                                                                                 │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 4-6 giờ phân loại & chuyển giao) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 4 (LLM trích xuất thực thể,      │
│ phân loại mức độ khẩn cấp, tự động route ticket và draft phản hồi ban đầu).     │
│                                                                                 │
│ Đo thành công bằng gì (Metric có số)?                                           │
│   - Thời gian phân loại & định tuyến giảm từ 4 giờ ──> dưới 30 giây.            │
│   - Tỉ lệ phân loại đúng bộ phận đạt trên 92%.                                  │
│                                                                                 │
│ Quick Architecture: [x] LLM Feature + Rule-based Dispatcher                     │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

### 🎴 Thẻ bài toán #3: Vinmec — Trợ lý tóm tắt hồ sơ bệnh án xuất viện lâm sàng

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                           │
│                                                                                 │
│ Bài toán (1 câu): Tự động trích xuất các chỉ số xét nghiệm, chẩn đoán và phác đồ │
│ điều trị từ bệnh án điện tử (EMR) để dự thảo Bản tóm tắt xuất viện cho bệnh nhân│
│ Công ty thành viên: [x] Vinmec                                                  │
│                                                                                 │
│ Ai đang đau (Actor)? Bác sĩ điều trị lâm sàng (quá tải thủ tục hành chính),     │
│                      Bệnh nhân (phải chờ đợi lâu để nhận giấy ra viện)          │
│                                                                                 │
│ Workflow thủ công hiện tại (5 bước):                                            │
│   1. Mở hồ sơ EMR ──> 2. Đọc tra cứu kết quả xét nghiệm/chuẩn đoán hình ảnh     │
│   ──> 3. Tóm lược diễn tiến bệnh ──> 4. Viết dặn dò thuốc ──> 5. Ký duyệt       │
│                                                                                 │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2, 3 & 4 (⏱ 25-30 phút/hồ sơ)             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 & 4 (LLM tổng hợp dữ liệu EMR,   │
│ draft tóm tắt lâm sàng chuẩn y khoa kèm giải thích dễ hiểu cho bệnh nhân).      │
│                                                                                 │
│ Đo thành công bằng gì (Metric có số)?                                           │
│   - Giảm thời gian bác sĩ soạn thảo từ 25 phút ──> dưới 5 phút.                 │
│   - 100% hồ sơ bắt buộc bác sĩ chuyên khoa ký duyệt (Zero unreviewed output).  │
│                                                                                 │
│ Quick Architecture: [x] LLM Feature (Strict Human-in-the-Loop)                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Đánh giá và lựa chọn bài toán Deep-Dive của nhóm

Nhóm quyết định chọn **Quick Problem Card #1: Xanh SM — Xử lý sự cố hết pin thực địa của tài xế taxi điện** để tiến hành phân tích sâu (Phase 3 Deep-Dive):
* **Tính cấp thiết vận hành (High Operational Impact):** Sự cố pin xe điện ảnh hưởng trực tiếp đến an toàn giao thông, thời gian chờ của hành khách và doanh thu của tài xế trong thời gian thực.
* **Độ khả thi kỹ thuật (High Feasibility):** Dữ liệu đầu vào rõ ràng (vị trí GPS xe, % pin, dữ liệu trạm sạc VinFast API).
* **Ranh giới an toàn rõ ràng (Strict Boundaries):** Dễ dàng thiết lập các rào cản an toàn nghiêm ngặt (Human-in-the-loop duyệt tin nhắn `[DRAFT_ONLY]`, ngưỡng pin `< 5%` bắt buộc kích hoạt `dispatch_mobile_charger`).
