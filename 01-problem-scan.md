# 01 — Problem Scan (Lab 02: AI Product Scoping)

**Học viên:** thaijaor
**Mã học viên:** 2A202602894
**Email:** thainguyenhong2204@gmail.com
**Branch:** `thai-02894`
**Ngày:** 2026-09-12

---

# 🔍 Phase 1 — SCAN

Quét hoạt động vận hành của các công ty thành viên Vingroup qua 4 lenses:
Repetitive / Time-consuming / AI-upgrade / Stakeholder Pain.

### 📝 List bài toán của tôi

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Xanh SM | Stakeholder Pain | Điều phối viên tổng đài phải **nghe và gõ tay** địa chỉ đón khách gọi qua hotline. Khách đọc địa chỉ kiểu dân gian ("ngõ 5 Trần Duy Hưng, cạnh quán phở Thìn"), điều phối viên tra bản đồ thủ công rồi mới gán tài xế. ⏱ ~3–4 phút/cuốc, tài xế phàn nàn điểm đón lệch phải gọi lại khách. |
| 2 | Vinhomes | Time-consuming | Ban quản lý toà nhà phải **soạn tay phản hồi** cho từng phàn nàn của cư dân trên app/group (tiếng ồn, thang máy, đỗ xe, phí dịch vụ). ⏱ ~10–12 phút/phản hồi, một toà 800 căn phát sinh ~40–60 phản hồi/ngày, SLA cam kết 24h thường bị vỡ vào cuối tuần. |
| 3 | VinFast | Repetitive | Nhân viên hậu kiểm phải **so khớp thủ công** hoá đơn sạc từ các trạm V-Green với log phiên sạc trong hệ thống để phát hiện sai lệch (sạc lỗi, trừ tiền 2 lần, phiên treo). ⏱ ~2 phút/hoá đơn × hàng chục nghìn phiên/ngày, đối soát cuối tháng kéo 3–5 ngày công. |
| 4 | Vinmec | AI-upgrade | Điều dưỡng tiếp nhận phải **đọc và tóm tắt tay** bệnh sử từ hồ sơ giấy/scan của bệnh nhân chuyển tuyến trước khi bác sĩ khám. ⏱ ~15–20 phút/hồ sơ, bác sĩ thường vẫn phải đọc lại vì bản tóm tắt thiếu thuốc đang dùng và tiền sử dị ứng. |
| 5 | Vinpearl / VinWonders | AI-upgrade | Tổng đài đặt phòng/vé trả lời khách quốc tế bằng **kịch bản có sẵn**, không xử lý được câu hỏi ghép nhiều ý ("phòng 2 người lớn 1 trẻ 5 tuổi ngày 3/10, có xe đón sân bay và vé VinWonders không?"). ⏱ khách chờ ~6–8 phút, tỉ lệ bỏ giỏ hàng cao vào mùa cao điểm. |

---

# 🃏 Phase 2 — QUICK-ASSESS

Chọn top 3 bài toán từ bảng trên: **#1 (Xanh SM)**, **#2 (Vinhomes)**, **#3 (VinFast)**.
Ba bài khác nhau về kiến trúc: Agent / LLM / Rule.

---

### QUICK PROBLEM CARD #1

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Điều phối viên Xanh SM phải nghe và gõ tay địa chỉ khách đọc bằng ngôn ngữ đời thường, dẫn tới điểm đón sai và tài xế phải gọi lại khách. |
| **Công ty thành viên** | ☑ Xanh SM |
| **Actor** | Điều phối viên tổng đài (ca 8h, ~120–150 cuộc/ca) |
| **Workflow thủ công** | 1. Nghe khách đọc địa chỉ → 2. Gõ tay vào ô tìm kiếm → 3. Tra bản đồ, đoán ngõ/ngách → 4. Xác nhận lại với khách → 5. Gán tài xế |
| **Bước tốn thời gian/lỗi nhất** | Bước 3 — tra và suy đoán địa chỉ mô tả theo mốc dân gian (⏱ ~2 phút/cuốc trong tổng 3–4 phút) |
| **AI hỗ trợ ở bước nào** | Bước 2–3: chuẩn hoá địa chỉ tự do thành địa chỉ có cấu trúc + toạ độ, đưa 2–3 ứng viên kèm độ tin cậy cho điều phối viên chọn |
| **Metric (có số)** | Giảm thời gian gán cuốc từ ~3,5 phút → dưới 1,5 phút; tỉ lệ tài xế phải gọi lại xác nhận điểm đón giảm từ ~20% → dưới 8% |
| **Quick Architecture** | ☑ Agent (LLM chuẩn hoá + gọi tool geocoding + vòng xác nhận với người) |

---

### QUICK PROBLEM CARD #2

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Ban quản lý Vinhomes soạn tay từng phản hồi cho phàn nàn của cư dân nên thường vỡ SLA 24h vào cuối tuần và cao điểm. |
| **Công ty thành viên** | ☑ Vinhomes |
| **Actor** | Nhân viên chăm sóc cư dân của ban quản lý toà nhà (~800 căn hộ) |
| **Workflow thủ công** | 1. Đọc phản ánh trên app/group → 2. Phân loại nhóm vấn đề → 3. Tra nội quy/lịch bảo trì → 4. Soạn phản hồi → 5. Gửi và theo dõi |
| **Bước tốn thời gian/lỗi nhất** | Bước 4 — soạn phản hồi (⏱ ~10–12 phút/ca, 40–60 ca/ngày) |
| **AI hỗ trợ ở bước nào** | Bước 2 và 4: tự phân loại + sinh bản nháp phản hồi bám nội quy, nhân viên chỉ sửa và duyệt |
| **Metric (có số)** | Thời gian soạn 1 phản hồi từ ~11 phút → dưới 3 phút; tỉ lệ phản hồi trong 24h từ ~70% → trên 95% |
| **Quick Architecture** | ☑ LLM (sinh bản nháp, người duyệt trước khi gửi) |

---

### QUICK PROBLEM CARD #3

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Nhân viên hậu kiểm VinFast đối soát tay hoá đơn sạc với log phiên sạc để tìm sai lệch, khiến chốt sổ cuối tháng kéo dài 3–5 ngày công. |
| **Công ty thành viên** | ☑ VinFast (V-Green) |
| **Actor** | Nhân viên hậu kiểm / kế toán vận hành trạm sạc |
| **Workflow thủ công** | 1. Xuất log phiên sạc → 2. Xuất giao dịch thanh toán → 3. So khớp từng dòng → 4. Đánh dấu lệch → 5. Lập phiếu hoàn tiền |
| **Bước tốn thời gian/lỗi nhất** | Bước 3 — so khớp thủ công (⏱ ~2 phút/hoá đơn nghi vấn) |
| **AI hỗ trợ ở bước nào** | Bước 4–5: phần lớn so khớp là rule thuần; AI chỉ xử lý nhóm lệch khó (phiên treo, trùng lặp) và viết diễn giải lý do cho phiếu hoàn tiền |
| **Metric (có số)** | Thời gian chốt đối soát tháng từ 3–5 ngày → dưới 1 ngày; tỉ lệ giao dịch phải xem tay giảm còn dưới 5% |
| **Quick Architecture** | ☑ Rule là chính (+ LLM phụ cho phần diễn giải) |
