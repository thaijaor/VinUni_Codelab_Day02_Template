# 01 — Problem Scan & Quick Problem Cards

> **Học viên thực hiện:** Hải (Branch: `hai-02482`)  
> **Đơn vị công tác:** AI Product Engineer tại Vin Smart Future (Vingroup)  
> **Nhiệm vụ:** Tìm kiếm, rà soát và đánh giá nhanh các cơ hội ứng dụng AI nhằm giải quyết các điểm nghẽn vận hành (Operational Bottlenecks) thực tế tại các công ty thành viên Vingroup.

---

# 🔍 Phase 1 — SCAN (Tìm kiếm cơ hội vận hành)

Sử dụng **4 Lenses** (Lặp lại, Tốn thời gian, AI có thể tốt hơn, Pain từ người khác) để quét qua các mảng vận hành của tập đoàn:

| # | Công ty thành viên (Subsidiary) | Lens | Mô tả ngắn bài toán vận hành & Điểm nghẽn |
|---|---------------------------------|------|-------------------------------------------|
| 1 | **Xanh SM (GSM)** | Tốn thời gian | **Xử lý sự cố sạc pin & cạn pin thực địa:** Tài xế gọi điện khẩn cấp báo pin dưới 5%; điều phối viên mất 15-20 phút tra cứu trạm sạc trống và điều phối xe sạc di động thủ công, dễ gây chết máy giữa đường. |
| 2 | **Vinhomes** | Lặp lại | **Phân loại & điều hướng phản ánh cư dân trên App:** Ban quản lý nhận hàng nghìn phản ánh/khiếu nại mỗi ngày (mất nước, rác, thang máy hỏng); nhân viên phải đọc từng tin nhắn và phân loại thủ công mất 4-6 tiếng. |
| 3 | **VinFast** | AI có thể tốt hơn | **Chẩn đoán sơ bộ mã lỗi xe từ mô tả tiếng Việt:** Khách hàng mô tả hiện tượng xe bằng ngôn ngữ tự nhiên (ví dụ: *"vào cua nghe tiếng lục cục ở bánh trước"*); hệ thống CSKH chưa tự map được sang mã lỗi kỹ thuật ban đầu. |
| 4 | **Vinmec** | Tốn thời gian | **Soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary):** Bác sĩ mất 20-30 phút/bệnh nhân để tổng hợp các xét nghiệm, bệnh án và thuốc điều trị thành tài liệu hướng dẫn dễ hiểu cho bệnh nhân xuất viện. |
| 5 | **Vinpearl** | Pain từ người khác | **Phân tích review & cảnh báo khiếu nại khẩn cấp:** Rà soát đánh giá của khách sạn trên Google Map, Agoda, Booking; các phàn nàn nghiêm trọng về vệ sinh hay thái độ phục vụ thường bị phát hiện trễ sau nhiều ngày. |
| 6 | **Xanh SM (GSM)** | Lặp lại | **Phân tích nguyên nhân hủy chuyến của khách hàng:** Tổng hợp dữ liệu hủy cuốc từ ghi âm tổng đài và ghi chú của tài xế để nhận diện các điểm nóng thiếu trạm sạc hoặc định vị GPS lệch. |

---

# 🃏 Phase 2 — QUICK-ASSESS (3 Quick Problem Cards)

Từ danh sách 6 bài toán trên, chọn lọc **Top 3 bài toán tiềm năng nhất** để phân tích đánh giá nhanh:

---

### 📇 QUICK PROBLEM CARD #1: Xanh SM — Xử lý sự cố sạc pin khẩn cấp thực địa

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                                  │
│                                                                                        │
│ Bài toán: Hỗ trợ điều phối viên phản hồi khẩn cấp và kích hoạt điều xe sạc lưu động   │
│           khi xe taxi Xanh SM báo động dung lượng pin dưới 5%.                         │
│ Công ty thành viên: [x] Xanh SM (GSM)     [ ] VinFast     [ ] Vinhomes                 │
│                     [ ] Vinmec            [ ] Vinpearl                                 │
│                                                                                        │
│ Ai đang đau (Actor)?                                                                   │
│ - Tài xế Xanh SM: Đang hoang mang vì xe sắp hết pin giữa đường, nguy cơ chết máy cao.   │
│ - Điều phối viên (Dispatcher): Quá tải vào giờ cao điểm, mất nhiều thời gian tra cứu.   │
│                                                                                        │
│ Workflow thủ công hiện tại (5 bước):                                                   │
│   1. Tài xế gọi hotline điều vận báo sự cố hết pin và đọc toạ độ/địa chỉ.              │
│   2. Điều phối viên tra cứu thủ công vị trí xe trên bản đồ GIS.                        │
│   3. Tra cứu tình trạng trụ sạc VinFast lân cận (còn cổng sạc trống hay không).        │
│   4. Soạn thảo tin nhắn hướng dẫn tài xế tới trạm hoặc liên hệ xe cứu hộ.              │
│   5. Gọi điện sang Đội xe sạc di động (Mobile Charging Van) để xuất kích.              │
│                                                                                        │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ Mất 12 - 15 phút/lượt).                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4.                                  │
│ - Tự động trích xuất vị trí, kiểm tra ngưỡng an toàn pin.                              │
│ - Tự động draft thông báo cứu hộ kèm thẻ [DRAFT_ONLY] cho điều phối viên duyệt trong 5s│
│                                                                                        │
│ Đo thành công bằng gì (Metric có số)?                                                  │
│ - Giảm thời gian xử lý sự cố từ 15 phút xuống dưới 2 phút.                             │
│ - Tỉ lệ xe điện chết máy giữa đường do cạn pin giảm 80%.                               │
│                                                                                        │
│ Quick Architecture: [ ] No AI     [ ] Rule     [x] LLM Feature     [ ] Multi-Agent     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #2: Vinhomes — Phân loại và điều hướng phản ánh cư dân

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                                  │
│                                                                                        │
│ Bài toán: Tự động tiếp nhận, trích xuất thực thể (phòng, tầng, vấn đề) và phân loại    │
│           phản ánh cư dân trên ứng dụng Vinhomes Resident về đúng bộ phận kỹ thuật.   │
│ Công ty thành viên: [ ] Xanh SM           [ ] VinFast     [x] Vinhomes                 │
│                     [ ] Vinmec            [ ] Vinpearl                                 │
│                                                                                        │
│ Ai đang đau (Actor)?                                                                   │
│ - Cư dân Vinhomes: Bực tức vì phản ánh mất nước, hỏng điều hòa chờ lâu không ai xử lý. │
│ - Ban Quản Lý (BQL): Quá tải khi phân loại 2.000+ phản ánh/ngày bằng mắt thường.       │
│                                                                                        │
│ Workflow thủ công hiện tại (4 bước):                                                   │
│   1. Cư dân gõ văn bản tự do gửi ticket khiếu nại trên app Vinhomes Resident.          │
│   2. Nhân viên lễ tân/CSKH đọc từng ticket, xác định mã căn hộ và tính chất sự cố.    │
│   3. Chuyển tiếp thủ công ticket sang Zalo/Hệ thống nội bộ của Đội Kỹ thuật tòa nhà.   │
│   4. Gọi điện thoại xác nhận lại với cư dân nếu mô tả không rõ ràng.                   │
│                                                                                        │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ Mất 20 - 45 phút để phân loại 1 ticket)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3.                                      │
│ - Phân loại intent (Điện/Nước/Vệ sinh/An ninh) và trích xuất độ khẩn cấp tự động.      │
│                                                                                        │
│ Đo thành công bằng gì (Metric có số)?                                                  │
│ - Thời gian phân loại và gán ticket giảm từ 30 phút xuống dưới 10 giây.                │
│ - Tỉ lệ phân loại chính xác đúng bộ phận đạt >= 92%.                                   │
│                                                                                        │
│ Quick Architecture: [ ] No AI     [ ] Rule     [x] LLM Feature     [ ] Multi-Agent     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

### 📇 QUICK PROBLEM CARD #3: VinFast — Trợ lý chẩn đoán mã lỗi sơ bộ cho Showroom/Xưởng dịch vụ

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                                  │
│                                                                                        │
│ Bài toán: Hỗ trợ Cố vấn Dịch vụ (Service Advisor) chuẩn đoán sơ bộ mã lỗi OBD-II và    │
│           phân loại cụm chi tiết hỏng từ mô tả hiện tượng xe của khách hàng.           │
│ Công ty thành viên: [ ] Xanh SM           [x] VinFast     [ ] Vinhomes                 │
│                     [ ] Vinmec            [ ] Vinpearl                                 │
│                                                                                        │
│ Ai đang đau (Actor)?                                                                   │
│ - Khách hàng sở hữu xe điện (VF5, VF8, VF9): Không rành kỹ thuật, giải thích lúng túng.│
│ - Cố vấn Dịch vụ: Mất nhiều thời gian hỏi đi hỏi lại và tra cứu sổ tay kỹ thuật dày cộp│
│                                                                                        │
│ Workflow thủ công hiện tại (4 bước):                                                   │
│   1. Khách hàng mang xe đến xưởng hoặc gọi điện thoại mô tả hiện tượng lạ.             │
│   2. Cố vấn dịch vụ ghi chép ra giấy, tra cứu tài liệu hướng dẫn kỹ thuật xưởng.       │
│   3. Phỏng đoán cụm chi tiết (Hệ thống treo, Pin cao áp, Mô-tơ, Phần mềm giải trí).    │
│   4. Bàn giao cho Kỹ thuật viên chính cắm máy đọc chẩn đoán chuyên dụng.               │
│                                                                                        │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ Mất 15 - 25 phút tư vấn ban đầu).      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3.                                      │
│ - Nhận dạng triệu chứng ngôn ngữ tự nhiên và đề xuất 3 nhóm nguyên nhân phổ biến nhất. │
│                                                                                        │
│ Đo thành công bằng gì (Metric có số)?                                                  │
│ - Rút ngắn thời gian tiếp nhận xe tại xưởng từ 25 phút xuống dưới 8 phút.               │
│ - Độ chính xác khoanh vùng nhóm lỗi đạt trên 85%.                                      │
│                                                                                        │
│ Quick Architecture: [ ] No AI     [ ] Rule     [x] LLM Feature     [ ] Multi-Agent     │
└────────────────────────────────────────────────────────────────────────────────────────┘
```
