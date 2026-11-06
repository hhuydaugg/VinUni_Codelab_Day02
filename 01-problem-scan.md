# 01 — Problem Scan & Quick Assess (Cá nhân)

**Học viên:** Nguyễn Hoàng Huy — 2A202603410
**Vai trò giả định:** AI Product Engineer tại Vin Smart Future

> ⚠️ **Ghi chú về số liệu:** Các con số thời gian/khối lượng dưới đây là **ước tính giả định** dựa trên quy trình điển hình của ngành (không phải số liệu nội bộ Vingroup). Chúng được dùng để so sánh mức độ ưu tiên giữa các bài toán và **cần xác minh bằng khảo sát thực địa / log hệ thống** trước khi đưa vào Deep-Dive.

---

## 🔍 Phase 1 — SCAN (4 Lenses)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** (Xưởng dịch vụ) | AI có thể tốt hơn | Cố vấn dịch vụ phải tự "dịch" mô tả lỗi bằng tiếng Việt đời thường của khách (*"đi qua gờ giảm tốc kêu cụp cụp bánh trước"*) sang nhóm lỗi kỹ thuật để mở phiếu sửa chữa; mô tả mơ hồ khiến kỹ thuật viên phải chẩn đoán lại từ đầu. |
| 2 | **Vinpearl** (Sales khách đoàn) | Tốn thời gian | Nhân viên sales đọc thủ công email đặt phòng theo đoàn của công ty lữ hành (nhiều ngày, nhiều loại phòng, yêu cầu đặc biệt nằm rải rác trong email dài), nhập lại vào PMS để kiểm tra quỹ phòng rồi soạn báo giá. |
| 3 | **Vinhomes** (Ban quản lý tòa nhà) | Lặp lại | Hàng trăm phản ánh/ngày trên app Vinhomes Resident (mất nước, hỏng đèn, tiếng ồn, phí dịch vụ...) được CSKH đọc và chuyển tay đến đúng bộ phận/tòa nhà; phân loại sai làm ticket bị "đá" qua lại. |
| 4 | **Xanh SM** (Trung tâm điều vận) | Pain từ người khác | Tài xế báo sự cố pin yếu/hết pin giữa đường; điều phối viên phải tra vị trí, tìm trạm sạc trống, soạn chỉ dẫn hoặc gọi cứu hộ — tài xế phàn nàn phải chờ lâu vào giờ cao điểm. *(Bài toán này được dùng cho phần prompt prototype.)* |
| 5 | **Vinmec** (Khối điều trị nội trú) | Tốn thời gian | Bác sĩ tổng hợp bệnh án, xét nghiệm, y lệnh để viết tóm tắt xuất viện và hướng dẫn cho bệnh nhân bằng ngôn ngữ dễ hiểu — việc giấy tờ chiếm nhiều thời gian cuối ca. |
| 6 | **VinWonders** (CSKH / Marketing) | Pain từ người khác | Review trên Google Maps/OTA về xếp hàng lâu, trò chơi bảo trì không báo trước... không được phát hiện và phản hồi kịp; quản lý khu vui chơi chỉ biết khi tổng hợp báo cáo cuối tuần. |

**Độ phủ lenses:** Lặp lại (#3), Tốn thời gian (#2, #5), AI có thể tốt hơn (#1), Pain từ người khác (#4, #6) — đủ cả 4 lenses, trải trên 6 công ty thành viên.

---

## 🃏 Phase 2 — QUICK-ASSESS: Top 3 Quick Problem Cards

**Tiêu chí chọn top 3:** (a) có xử lý ngôn ngữ tự nhiên thật sự — nơi LLM vượt trội rule-based; (b) rủi ro khi AI sai có thể kiểm soát bằng human review; (c) có metric đo được ngay.
**Loại ra:** #4 (đã có worked example của lớp và dùng làm prototype), #5 (rủi ro y khoa cao, cần bác sĩ duyệt 100% → lợi ích thời gian thấp ở giai đoạn đầu), #6 (bài toán offline, ít cấp bách).

### Card #1 — VinFast: Chuẩn hóa mô tả lỗi xe của khách thành phiếu chẩn đoán sơ bộ

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Chuyển mô tả lỗi tiếng Việt tự do của khách thành │
│ nhóm lỗi kỹ thuật + câu hỏi làm rõ cho phiếu tiếp nhận xe.  │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Cố vấn dịch vụ (SA) tại xưởng; kỹ      │
│ thuật viên nhận phiếu mơ hồ; khách phải kể lại nhiều lần.   │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Khách gọi hotline/đến xưởng mô tả lỗi bằng lời         │
│   → 2. SA ghi chú tay, hỏi thêm theo kinh nghiệm cá nhân    │
│   → 3. SA chọn nhóm lỗi trong DMS & mở phiếu sửa chữa       │
│   → 4. KTV đọc phiếu, chạy chẩn đoán, gọi lại SA/khách nếu  │
│        thông tin thiếu                                      │
│                                                             │
│ Bước tốn thời gian/lỗi nhất? Bước 2–3 (⏱ ~10 phút/xe) và    │
│ vòng hỏi lại ở bước 4 (~15% phiếu, ⏱ ~20 phút/lượt)          │
│ AI hỗ trợ ở bước nào? Bước 2–3: LLM đọc mô tả → gợi ý top-3  │
│ nhóm lỗi + checklist câu hỏi làm rõ; SA xác nhận.           │
│                                                             │
│ Metric: Giảm thời gian tiếp nhận từ ~10 phút → < 4 phút/xe; │
│ giảm tỉ lệ phiếu phải hỏi lại từ ~15% → < 5%; top-3 gợi ý   │
│ chứa đúng nhóm lỗi (theo KTV xác nhận) ≥ 85%.               │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │
└─────────────────────────────────────────────────────────────┘
```

**Ranh giới:** AI chỉ *gợi ý* nhóm lỗi, **không** kết luận chẩn đoán, không báo giá, không tư vấn "xe vẫn chạy được" với lỗi liên quan phanh/pin cao áp/lái — các từ khóa an toàn này luôn bị đẩy lên cờ đỏ để SA xử lý.

**Tự phản biện (CFO/Trưởng vận hành):** *"Một cây quyết định từ khóa (kêu, rung, cảnh báo pin...) có rẻ hơn không?"* → Rule-based làm được 60–70% ca rõ ràng, nhưng mô tả của khách rất đa dạng (tiếng địa phương, ẩn dụ "kêu cụp cụp", "xe ì"), đây đúng là chỗ LLM mạnh. Phương án hợp lý: **rule cho từ khóa an toàn + LLM cho phần mô tả tự do**.

---

### Card #2 — Vinpearl: Trích xuất yêu cầu đặt phòng khách đoàn từ email

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Đọc email group booking dài/không chuẩn của công  │
│ ty lữ hành và trích xuất thành yêu cầu đặt phòng có cấu trúc│
│ Công ty thành viên: [x] Khác: Vinpearl                      │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Sales khách đoàn; đối tác lữ │
│ hành chờ báo giá; Revenue Manager thiếu dữ liệu kịp thời.   │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Nhận email (thường kèm bảng/tệp, nhiều lần sửa đổi)    │
│   → 2. Đọc & ghi lại: ngày, số phòng, loại phòng, bữa ăn,   │
│        yêu cầu đặc biệt                                     │
│   → 3. Kiểm tra quỹ phòng trên PMS                          │
│   → 4. Soạn email báo giá / đề xuất phương án thay thế      │
│                                                             │
│ Bước tốn thời gian/lỗi nhất? Bước 2 (⏱ ~20–25 phút/email),  │
│ hay sót yêu cầu đặc biệt nằm cuối email hoặc ở thư trả lời. │
│ AI hỗ trợ ở bước nào? Bước 2 (trích xuất JSON) và bước 4    │
│ (draft email báo giá từ kết quả PMS).                       │
│                                                             │
│ Metric: Thời gian phản hồi đối tác từ ~4 giờ → < 1 giờ;      │
│ thời gian xử lý bước 2 từ ~20 phút → < 5 phút; độ chính xác │
│ trích xuất từng trường (ngày, số phòng, loại phòng) ≥ 95%.  │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │
└─────────────────────────────────────────────────────────────┘
```

**Ranh giới:** AI **không** tự giữ phòng (hold) trên PMS, không tự đưa ra giá/chiết khấu, không gửi email cho đối tác — mọi email là bản nháp chờ Sales duyệt. Trường nào AI không chắc chắn phải để `null` + cờ "cần xác nhận", không được đoán.

**Tự phản biện:** *"Sao không bắt đối tác điền form chuẩn?"* → Đúng, đây là giải pháp No-AI rẻ nhất và nên song song triển khai với đối tác lớn; nhưng thực tế đa số đối tác nhỏ vẫn gửi email tự do nên LLM extraction vẫn có giá trị với phần đuôi dài này.

---

### Card #3 — Vinhomes: Phân loại & định tuyến phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tự động phân loại phản ánh của cư dân trên app    │
│ Vinhomes Resident và chuyển đúng bộ phận + mức độ khẩn cấp. │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? CSKH ban quản lý (đọc & chuyển ticket);│
│ cư dân (chờ lâu); kỹ thuật tòa nhà (nhận ticket sai việc).  │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Cư dân gửi phản ánh (text + ảnh) trên app              │
│   → 2. CSKH đọc, xác định loại việc & tòa/căn               │
│   → 3. Chuyển ticket đến bộ phận (kỹ thuật/an ninh/vệ sinh/ │
│        kế toán)                                             │
│   → 4. Bộ phận nhận, nếu sai việc thì trả lại CSKH          │
│   → 5. Phản hồi cư dân                                      │
│                                                             │
│ Bước tốn thời gian/lỗi nhất? Bước 2–4: ⏱ ~5 phút/ticket để  │
│ đọc & chuyển, nhưng thời gian chờ trong hàng đợi ngoài giờ  │
│ hành chính lên tới vài giờ; ~10–15% ticket bị chuyển sai.   │
│ AI hỗ trợ ở bước nào? Bước 2–3: LLM phân loại loại việc +   │
│ mức khẩn cấp; ticket khẩn (rò nước, kẹt thang máy) được đẩy │
│ thẳng lên trực ca.                                          │
│                                                             │
│ Metric: 85% ticket được phân loại & định tuyến < 1 phút;    │
│ tỉ lệ chuyển sai bộ phận từ ~12% → < 5%; 100% ticket khẩn   │
│ cấp được báo trực ca < 5 phút.                              │
│                                                             │
│ Quick Architecture: [ ] No AI [x] Rule [x] LLM [ ] Agent    │
└─────────────────────────────────────────────────────────────┘
```

**Ranh giới:** AI **không** trả lời cư dân về phí dịch vụ, tranh chấp, pháp lý; không hứa thời gian sửa chữa; độ tin cậy thấp → về hàng đợi CSKH như hiện tại (fallback). Từ khóa an toàn (cháy, khói, kẹt thang máy, rò điện) do **rule** bắt trước, không phụ thuộc LLM.

**Tự phản biện:** *"App đã có dropdown chọn loại phản ánh, sao cần AI?"* → Nếu cư dân chọn đúng loại thì rule là đủ; cần đo trước tỉ lệ cư dân chọn sai/chọn "Khác". Nếu tỉ lệ này thấp, bài toán nên là **NO-GO cho LLM** và chỉ cần sửa UX.

---

## 🗳️ Đề xuất mang vào thảo luận nhóm

Đề xuất **Card #1 (VinFast — chuẩn hóa mô tả lỗi xe)** cho Deep-Dive vì:
1. Là bài toán ngôn ngữ tự nhiên thực thụ — nơi rule-based yếu nhất và LLM có lợi thế rõ.
2. Ranh giới an toàn rõ ràng (AI chỉ gợi ý, SA và KTV luôn xác nhận) → rủi ro kiểm soát được qua HITL.
3. Metric đo được ngay từ dữ liệu DMS sẵn có (thời gian mở phiếu, tỉ lệ phiếu phải hỏi lại, nhóm lỗi cuối cùng KTV xác nhận).

Card #2 là phương án dự phòng tốt (rủi ro thấp, ROI dễ chứng minh). Card #3 cần đo baseline tỉ lệ chọn sai dropdown trước khi quyết định có cần LLM hay không.
