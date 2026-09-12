# 01 — Problem Scan (Bản tổng hợp nhóm)

Tổng hợp từ bản scan cá nhân của 5 thành viên, giữ lại các bài toán được nhiều
người cùng phát hiện.

---

## Phase 1 — SCAN

| # | Subsidiary | Lens | Bài toán & điểm nghẽn | Số thành viên cùng nêu |
|---|---|---|---|:-:|
| 1 | Xanh SM | Tốn thời gian | Điều phối viên xử lý thủ công báo cáo pin cạn (< 5%) từ tài xế: tra GPS, dò trụ sạc còn trống, soạn tin hướng dẫn, gọi xe sạc lưu động. ⏱ 12–15 phút/lượt, xe có nguy cơ chết máy giữa đường. | 4/5 |
| 2 | Vinhomes | Lặp lại | Phản ánh cư dân trên app (mất nước, thang máy, tiếng ồn, an ninh) được đọc và phân loại thủ công rồi chuyển tay tới ban quản lý. ⏱ 2–6 tiếng/đợt, chuyển sai bộ phận phải định tuyến lại. | 5/5 |
| 3 | VinFast | Lặp lại | Đối soát log phiên sạc tại trạm với hóa đơn/doanh thu đối tác để phát hiện sai lệch (phiên treo, trừ tiền trùng). Hàng chục nghìn dòng/tuần, làm tay. | 4/5 |
| 4 | Vinmec | Tốn thời gian | Bác sĩ tổng hợp xét nghiệm, diễn tiến điều trị và toa thuốc từ EMR thành bản tóm tắt xuất viện dễ hiểu cho bệnh nhân. ⏱ 20–30 phút/bệnh nhân. | 4/5 |

---

## Bài toán nhóm chọn: #1 — Xanh SM, điều phối sự cố pin cạn

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Hỗ trợ điều phối viên Xanh SM phản hồi nhanh khi tài xế báo pin dưới 5%, và kích hoạt xe sạc lưu động thay vì chỉ tài xế tới trạm sạc ở xa. |
| **Actor** | Điều phối viên tổng đài (quá tải giờ cao điểm) và tài xế đang có nguy cơ chết máy giữa đường. |
| **Workflow thủ công** | 1. Tài xế gọi hotline báo pin cạn, đọc vị trí → 2. Điều phối viên tra GPS trên bản đồ → 3. Dò trạm sạc lân cận còn trụ trống → 4. Soạn tin hướng dẫn → 5. Gọi đội xe sạc lưu động nếu cần. |
| **Bottleneck** | Bước 3–4: dò trạm và soạn tin thủ công, ⏱ 12–15 phút/lượt. |
| **AI hỗ trợ ở đâu** | Bước 2–4: trích xuất vị trí và mức pin, kiểm tra ngưỡng an toàn, sinh bản nháp tin nhắn kèm tag `[DRAFT_ONLY]` để điều phối viên duyệt. |
| **Success Metric** | Thời gian xử lý một sự cố từ ~15 phút xuống dưới 2 phút; 100% bản nháp giữ tag `[DRAFT_ONLY]` trước khi gửi. |
| **Quick Architecture** | Agentic Loop — LLM + tool tra cứu trạm sạc + Human-in-the-loop bắt buộc. |
| **Operational Boundary** | Pin < 5%: cấm đề xuất trạm xa hơn 5km, phải trả `{"action": "dispatch_mobile_charger"}`. AI không được tự gửi tin, không huỷ cuốc, không hứa bồi thường. |

### Vì sao chọn bài toán này

- Được 4/5 thành viên độc lập phát hiện, chứng tỏ điểm nghẽn rõ.
- Có ranh giới an toàn định lượng được (ngưỡng pin 5%, bán kính 5km) nên stress-test được bằng prompt prototype.
- Bắt buộc có bước người duyệt, phù hợp yêu cầu Human-in-the-loop của bài lab.
