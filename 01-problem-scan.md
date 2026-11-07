# 01 — Problem Scan & Quick Assess (Cá nhân)

**Học viên:** Hồ Xuân Vũ — 2A202603406  
**Vai trò giả định:** AI Product Engineer tại Vin Smart Future  

> ⚠️ **Ghi chú về số liệu:** Các con số thời gian/khối lượng dưới đây là **ước tính giả định** dựa trên quy trình điển hình của ngành (không phải số liệu nội bộ Vingroup). Chúng được dùng để so sánh mức độ ưu tiên giữa các bài toán và **cần xác minh bằng khảo sát thực địa / log hệ thống** trước khi đưa vào Deep-Dive.

---

## 🔍 Phase 1 — SCAN (4 Lenses)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **VinFast** (Xưởng dịch vụ) | AI có thể tốt hơn | **Thẩm định khiếu nại bảo hành pin và telemetry:** Kỹ thuật viên phải đối chiếu hàng gigabyte dữ liệu telemetry từ xe với lịch sử sạc để xác định nguyên nhân lỗi. Thời gian xét duyệt kéo dài 7–14 ngày/ca, khiến tỷ lệ khiếu nại không hài lòng tăng 22% và rủi ro bồi thường sai/chậm gây thiệt hại ước tính ~15–20 tỷ đồng/năm. |
| 2 | **Xanh SM** (Trung tâm điều vận) | Pain từ người khác | **Điều phối và dự báo cung - cầu xe giờ cao điểm/trời mưa:** Việc phân bổ xe dựa trên kinh nghiệm điều phối viên hoặc quy tắc tĩnh, không dự báo kịp thời điểm nghẽn cục bộ. Gây ra tình trạng ùn ứ điểm nóng (sân bay, trung tâm thương mại), tỷ lệ hủy chuyến tăng 18% và lãng phí ~15% năng suất tài xế do chạy xe không khách tìm điểm đón. |
| 3 | **Vinhomes** (Ban quản lý tòa nhà) | Lặp lại | **Phân loại và định tuyến ticket sự cố kỹ thuật tòa nhà:** Tiếp nhận và phân loại hàng nghìn yêu cầu bảo trì (thang máy, PCCC, điện nước cư dân) qua app/hotline bằng nhân sự thủ công. Tỷ lệ phân bổ nhầm đội ngũ kỹ thuật chuyên môn chiếm 25%, kéo dài thời gian phản hồi trung bình lên 3-4 giờ và làm sụt giảm chỉ số hài lòng (CSAT) cư dân. |
| 4 | **Vinmec** (Khối điều trị nội trú) | Tốn thời gian | **Sàng lọc nhanh chẩn đoán hình ảnh và cấu trúc hóa bệnh án:** Bác sĩ mất nhiều thời gian đọc bản scan phức tạp (X-quang, MRI) và viết báo cáo tóm tắt thủ công từ hệ thống EHR rời rạc. Gây chậm trễ trong các ca cấp cứu/hội chẩn chuyên sâu, tăng nguy cơ quá tải ca trực ban lên 30% và bỏ sót các dấu hiệu bất thường nhỏ giai đoạn đầu. |
| 5 | **VinFast** (Supply Chain) | Tốn thời gian | **Dự báo tồn kho linh kiện thay thế tại xưởng dịch vụ:** Quản lý kho phụ tùng dựa trên định mức cố định thay vì dự báo thời gian hỏng hóc thực tế của xe lưu thông trên đường. Gây ra hiện tượng thiếu linh kiện phổ biến (làm giam xe khách 2-3 tuần) song song với việc tồn đọng hàng trăm tỷ đồng phụ tùng ít dùng, gây thất thoát chi phí lưu kho và đền bù hợp đồng. |

**Độ phủ lenses:** Lặp lại (#3), Tốn thời gian (#4, #5), AI có thể tốt hơn (#1), Pain từ người khác (#2) — đầy đủ các khía cạnh lenses trọng yếu, trải dài trên các công ty thành viên cốt lõi của Vingroup.

---

## 🃏 Phase 2 — QUICK-ASSESS: Top 3 Quick Problem Cards

**Tiêu chí chọn top 3:** (a) Có xử lý dữ liệu phức tạp/NLP/mô hình dự báo — nơi AI vượt trội rule-based; (b) Rủi ro khi AI sai có thể kiểm soát bằng Human-in-the-loop (HITL); (c) Có metric đo lường hiệu quả rõ ràng và trực tiếp.  
**Loại ra:** #2 (bài toán tối ưu hóa bài toán xe chạy thực tế phức tạp, rủi ro điều vận cao), #4 (rủi ro y khoa cao, phụ thuộc tuyệt đối vào xác nhận của bác sĩ → tối ưu thời gian giai đoạn đầu chưa đạt kỳ vọng ROI ngay).

---

### Card #1 — VinFast: Thẩm định tự động dữ liệu telemetry & lịch sử sạc pin EV

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Trích xuất, đối chiếu telemetry và chẩn đoán      │
│ nguyên nhân suy giảm dung lượng/lỗi sạc pin xe điện.        │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Kỹ thuật viên bảo hành (đọc dữ liệu    │
│ lớn); khách hàng (chờ duyệt 7–14 ngày); Quản lý chi phí.    │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Tiếp nhận khiếu nại pin/sạc & trích xuất log telemetry │
│   → 2. Kỹ thuật viên lọc thủ công file log (V, I, Temp, SOC)│
│   → 3. Đối chiếu lịch sử sạc trụ public/home charge        │
│   → 4. Viết báo cáo thẩm định & trình quản lý duyệt bồi thường│
│                                                             │
│ Bước tốn thời gian/lỗi nhất? Bước 2–3 (⏱ ~7–14 ngày/ca) và  │
│ sai sót đánh giá thủ công gây đền bù sai (~15–20 tỷ/năm).   │
│ AI hỗ trợ ở bước nào? Bước 2–3: Anomaly Detection + LLM     │
│ Agent đọc log, đối chiếu thông số & xuất draft báo cáo      │
│ thẩm định kèm mức độ tin cậy; KTV duyệt xác nhận.            │
│                                                             │
│ Metric: Giảm thời gian xử lý từ 7–14 ngày → < 24 giờ/ca;    │
│ giảm tỷ lệ bồi thường sai sót < 2%; độ chính xác gợi ý    │
│ nguyên nhân gốc (Root-cause analysis) ≥ 90%.                │
│                                                             │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [x] Agent    │
└─────────────────────────────────────────────────────────────┘
```

**Ranh giới:** AI chỉ đưa ra *gợi ý nguyên nhân & đề xuất phương án bồi thường*, **không** tự động phê duyệt quyết định chi tiền bồi thường hoặc thay pin mới — quyết định cuối cùng luôn thuộc về KTV Trưởng/Quản lý bảo hành.

**Tự phản biện:** *"Một hệ thống Threshold-rule (nếu nhiệt độ > 60°C thì cờ đỏ) có đủ không?"* → Rule chỉ phát hiện được sự cố ngưỡng đơn giản, không phân tích được mẫu (pattern) thoái hóa pin kéo dài kết hợp với thói quen sạc. **Sự kết hợp giữa Anomaly Detection + LLM Agent** giúp đọc chuỗi thời gian (time-series data) và tổng hợp thành báo cáo diễn giải tự nhiên cho kỹ thuật viên.

---

### Card #2 — Vinhomes: Tự động phân loại & định tuyến phản ánh kỹ thuật tòa nhà

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Phân loại thông minh mô tả sự cố của cư dân trên  │
│ app Vinhomes Resident và định tuyến đến đúng đội kỹ thuật. │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? CSKH Ban quản lý (tốn giờ đọc/chuyển); │
│ Cư dân (chờ xử lý lâu); Kỹ thuật viên (nhận sai công việc). │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Cư dân gửi phản ánh (text/hình ảnh) trên app           │
│   → 2. CSKH đọc, xác định nhóm sự cố (điện, nước, PCCC...)  │
│   → 3. Giao ticket thủ công cho đội kỹ thuật ca trực        │
│   → 4. Đội kỹ thuật tiếp nhận (nếu sai chuyên môn thì trả   │
│      lại để CSKH chuyển lại)                                │
│                                                             │
│ Bước tốn thời gian/lỗi nhất? Bước 2–4: Tỷ lệ phân bổ nhầm    │
│ chiếm 25%, kéo dài thời gian phản hồi trung bình 3-4 giờ.   │
│ AI hỗ trợ ở bước nào? Bước 2–3: Multimodal LLM phân tích   │
│ hình ảnh + văn bản → phân loại sự cố và tự định tuyến        │
│ ticket đến đúng ca trực kỹ thuật chuyên môn.               │
│                                                             │
│ Metric: Phân loại & định tuyến ticket tự động < 1 phút;     │
│ giảm tỷ lệ giao sai đội ngũ kỹ thuật từ 25% → < 5%;        │
│ rút ngắn thời gian phản hồi ban đầu xuống < 30 phút.        │
│                                                             │
│ Quick Architecture: [ ] No AI [x] Rule [x] LLM [ ] Agent    │
└─────────────────────────────────────────────────────────────┘
```

**Ranh giới:** AI **không** tự trả lời các cam kết về tài chính, tranh chấp hợp đồng hoặc từ chối trách nhiệm. Đối với các từ khóa rủi ro an toàn cao (cháy nổ, mắc kẹt thang máy, rò rỉ khí gas), hệ thống kích hoạt **Rule cảnh báo khẩn cấp (Emergency Rule)** để chuyển thẳng sang cuộc gọi báo động ca trực khẩn cấp.

**Tự phản biện:** *"Nếu cư dân chọn nhầm danh mục trên UI thì sao?"* → Đa số cư dân thường chọn mục "Khác" hoặc chọn đại cho xong. AI đọc trực tiếp hình ảnh (rò rỉ ống nước, vỡ gạch, hỏng bóng đèn) và văn bản tự do để sửa lại danh mục đúng, giải quyết vấn đề UX thụ động từ phía người dùng.

---

### Card #3 — VinFast (Supply Chain): Dự báo tồn kho linh kiện thay thế tại xưởng dịch vụ

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Dự báo nhu cầu phụ tùng thay thế dựa trên dữ liệu │
│ hỏng hóc thực tế và lịch trình bảo dưỡng của xe lưu thông.   │
│ Công ty thành viên: [x] VinFast (Supply Chain)              │
│                                                             │
│ Ai đang đau (Actor)? Quản lý kho phụ tùng; Cố vấn dịch vụ  │
│ (thiếu linh kiện sửa cho khách); Khách hàng (xe bị giam lâu).│
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Đặt hàng phụ tùng dựa trên định mức cố định hằng tháng │
│   → 2. Xe vào xưởng phát sinh lỗi thiếu phụ tùng đột xuất   │
│   → 3. Quản lý kho điều chuyển gấp từ kho tổng/xưởng khác   │
│   → 4. Tồn đọng phụ tùng ít dùng nhưng thiếu phụ tùng hot   │
│                                                             │
│ Bước tốn thời gian/lỗi nhất? Bước 1 & 3: Xe bị om 2-3 tuần  │
│ do chờ phụ tùng; tồn kho bất hợp lý lên tới trăm tỷ đồng.  │
│ AI hỗ trợ ở bước nào? Bước 1: Predictive Analytics (ML)     │
│ phân tích ODO, độ tuổi xe trên địa bàn và dữ liệu hỏng hóc  │
│ lịch sử để lập kế hoạch tồn kho tối ưu theo từng xưởng.     │
│                                                             │
│ Metric: Giảm tỷ lệ thiếu linh kiện tại xưởng từ ~20% → < 5%;│
│ giảm thời gian chờ sửa chữa của khách từ 2-3 tuần → < 3 ngày;│
│ giảm chi phí lưu kho phụ tùng tồn đọng 15%.                 │
│                                                             │
│ Quick Architecture: [x] Predictive ML [ ] Rule [ ] LLM     │
└─────────────────────────────────────────────────────────────┘
```

**Ranh giới:** AI đưa ra *danh mục và số lượng phụ tùng gợi ý luân chuyển/đặt hàng*, **không** tự động phát hành lệnh mua hàng (Purchase Order) có giá trị lớn mà không qua phê duyệt của Giám đốc Chuỗi cung ứng.

**Tự phản biện:** *"Có nên dùng LLM ở bài toán này không?"* → Không, đây là bài toán Time-Series & Tabular Data đặc trưng. Việc sử dụng các mô hình Machine Learning truyền thống (XGBoost, Prophet, LSTM) sẽ mang lại độ chính xác cao hơn, tiết kiệm chi phí vận hành hơn so với việc cố ép LLM vào làm.

---

## 🗳️ Đề xuất mang vào thảo luận nhóm

Đề xuất **Card #1 (VinFast — Thẩm định telemetry & bảo hành pin EV)** cho Deep-Dive vì:
1. **Giá trị kinh tế lớn:** Trực tiếp giải quyết bài toán rò rỉ chi phí bồi thường (15-20 tỷ/năm) và nâng cao chỉ số hài lòng của khách hàng dùng xe điện VinFast.
2. **Công nghệ phù hợp:** Phối hợp tốt giữa phân tích dữ liệu telemetry kỹ thuật (Anomaly Detection) và năng lực tổng hợp/diễn giải báo cáo của LLM Agent.
3. **Tính khả thi & An toàn:** Dữ liệu log có sẵn trên hệ thống connected car; mô hình vận hành theo cơ chế **Human-in-the-loop** (AI hỗ trợ, KTV quyết định) giúp triệt tiêu rủi ro sai sót.

*Card #2 là phương án dự phòng tốt về mặt tối ưu vận hành CSKH. Card #3 dành cho hướng tối ưu hóa chuỗi cung ứng chuyên sâu.*