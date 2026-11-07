# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý và chuyển đổi hồ sơ bệnh án điện tử (EMR) tại hệ thống bệnh viện & phòng khám.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho bài toán **chuyển đổi và vận hành Bệnh án điện tử (EMR) tại hệ thống Phòng khám / Vinmec**.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — Worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành chuyển đổi và chuẩn hóa Bệnh án điện tử (EMR) tại hệ thống Phòng khám Vinmec (và các công ty thành viên Vingroup). Ghi lại **5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ hiện tại còn chậm, dễ bỏ sót trường thông tin hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến bệnh nhân, bác sĩ hoặc điều dưỡng phòng khám phàn nàn.

### 📝 List bài toán của tôi:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| **01** | **Vinmec (Hệ thống Phòng khám)** | **Tốn thời gian** | **Rà hồ sơ đang dùng:** Đối chiếu thủ công hàng trăm mẫu bệnh án giấy/Word, biểu mẫu chuyên khoa và trường dữ liệu hiện có với cấu trúc bệnh án điện tử (EMR) cần chuyển đổi (mất 25–30 phút/biểu mẫu, dễ sót trường bắt buộc theo quy định Bộ Y tế). |
| **02** | **Vinmec (Hệ thống Phòng khám)** | **Lặp lại** | **Phân việc trong phòng khám:** Rà soát và theo dõi thủ công trạng thái hồ sơ giữa các vai trò (ai ghi, ai kiểm tra, ai ký số, ai đôn đốc hồ sơ treo/chưa xong) vào cuối mỗi ca khám, gây ứ đọng 15–20% hồ sơ chưa hoàn tất trong ngày. |
| **03** | **Vinmec (Hệ thống Phòng khám)** | **AI-upgrade** | **Chọn chữ ký số, Zalo ZNS & hạ tầng lưu trữ:** Đánh giá, đối chiếu thông số kỹ thuật/bảo mật của các nhà cung cấp chữ ký số (HSM/Remote Signing), mẫu thông báo Zalo ZNS gửi bệnh nhân, thiết bị phần cứng và nơi lưu dữ liệu (Cloud/On-prem) còn rời rạc, thiếu công cụ chuẩn hóa cấu hình và soạn template ZNS tự động. |
| **04** | **Vinmec (Hệ thống Phòng khám)** | **Pain từ người khác** | **Làm thử với ca thật (Lập, bổ sung, ký và xuất bệnh án):** Khi vận hành ca khám thật, bác sĩ và điều dưỡng mất 15–20 phút/ca để lập, rà soát nội dung còn thiếu (ICD-10, sinh hiệu, tóm tắt điều trị), trình ký số và xuất bệnh án/gửi Zalo ZNS, khiến bác sĩ quá tải và bệnh nhân phải chờ lấy hồ sơ lâu. |
| **05** | **Vinhomes / Xanh SM** | **Lặp lại** | **Đối soát & phân loại phản hồi dịch vụ đa kênh:** Nhân viên vận hành phải đọc và phân loại thủ công hàng trăm yêu cầu hỗ trợ kỹ thuật/hóa đơn mỗi ngày trước khi chuyển đúng bộ phận xử lý. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** trọng tâm từ chuỗi chuyển đổi & vận hành phòng khám (**#01 Rà hồ sơ đang dùng**, **#02 Phân việc & theo dõi hồ sơ**, và **#04 Làm thử với ca thật: lập, bổ sung, ký và xuất bệnh án**) để hoàn thiện **3 Quick Problem Cards** dưới đây.

### Card #1 — Rà hồ sơ đang dùng (Đối chiếu bệnh án & biểu mẫu sang EMR)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #01                                      │
│                                                             │
│ Bài toán (1 câu): Đối chiếu bệnh án, biểu mẫu và dữ liệu    │
│ hiện có của phòng khám với chuẩn dữ liệu EMR cần chuyển.    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec (Chuỗi Phòng khám)           │
│                                                             │
│ Ai đang đau (Actor)? Chuyên viên КHTH / Quản trị dữ liệu     │
│ phòng khám & Bác sĩ trưởng phòng khám.                      │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Thu thập mẫu bệnh án, phiếu chỉ định, biểu mẫu đang dùng│
│   ──> 2. Đọc & liệt kê từng trường thông tin vào file Excel │
│   ──> 3. Đối chiếu từng trường với danh mục chuẩn EMR       │
│   ──> 4. Ghi chú phần thiếu/lệch để yêu cầu IT cấu hình lại │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 25 phút/mẫu) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (Trích xuất trường từ biểu mẫu cũ & gợi ý mapping sang EMR) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm thời gian đối chiếu 1 biểu mẫu từ 25 min ──> 4 min, │
│    phát hiện đúng >= 95% trường dữ liệu bị thiếu/lệch."     │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Card #2 — Phân việc trong phòng khám & Theo dõi hồ sơ chưa xong

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #02                                      │
│                                                             │
│ Bài toán (1 câu): Thống nhất và kiểm soát luồng ai ghi,     │
│ ai kiểm tra, ai ký và ai theo dõi hồ sơ bệnh án chưa xong.  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec (Chuỗi Phòng khám)           │
│                                                             │
│ Ai đang đau (Actor)? Điều dưỡng, Bác sĩ khám, Nhân viên     │
│ kiểm tra hồ sơ (KHTH) và Quản lý phòng khám.                │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Điều dưỡng ghi nhận hành chính & sinh hiệu ban đầu     │
│   ──> 2. Bác sĩ khám, ghi chẩn đoán/điều trị trên phần mềm  │
│   ──> 3. NV kiểm tra mở từng hồ sơ rà lỗi trống trường/chưa ký│
│   ──> 4. Nhắn tin thủ công nhắc từng bác sĩ bổ sung & ký số │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10 phút/ca)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4            │
│ (Tự động rà lỗi logic lâm sàng-hành chính & phân luồng nhắc │
│ đúng người chịu trách nhiệm: người ghi vs người kiểm vs ký) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm tỉ lệ hồ sơ treo qua ngày từ 18% ──> dưới 2%,       │
│    giảm thời gian rà soát phân việc từ 10 min ──> 1 min/ca" │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Card #3 — Làm thử với ca thật: Lập, bổ sung, ký số và xuất bệnh án (Kết hợp 01–04)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #04                                      │
│                                                             │
│ Bài toán (1 câu): Hỗ trợ phòng khám vận hành trơn tru các   │
│ bước lập, bổ sung, kiểm tra, ký số và xuất bệnh án (kèm     │
│ thông báo Zalo ZNS) trên ca bệnh thật.                      │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec (Chuỗi Phòng khám)           │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị, Điều dưỡng, Nhân viên │
│ KHTH kiểm duyệt và Bệnh nhân chờ kết quả xuất viện/ra về.   │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Điều dưỡng lập hồ sơ, nhập sinh hiệu & lý do khám      │
│   ──> 2. Bác sĩ ghi bệnh sử, khám lâm sàng, chẩn đoán, thuốc│
│   ──> 3. NV kiểm tra đối chiếu biểu mẫu, yêu cầu bổ sung    │
│   ──> 4. Bác sĩ ký số (CA) và xuất file bệnh án điện tử     │
│   ──> 5. Soạn tóm tắt dặn dò gửi qua Zalo ZNS cho bệnh nhân │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2, 3 & 5 (⏱ 14 phút)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 & 5         │
│ (Đối chiếu thiếu sót hồ sơ theo vai trò, chuẩn hóa tóm tắt  │
│ bệnh án trước khi ký số, và soạn nháp tin nhắn Zalo ZNS)    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm thời gian hoàn tất & xuất bệnh án từ 18 min ──>     │
│    dưới 4 min/ca; 100% hồ sơ đủ điều kiện trước khi bấm ký" │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

> **Bài toán nhóm lựa chọn Deep-Dive:** **Trợ lý AI Điều phối & Chuẩn hóa Hồ sơ Bệnh án Điện tử (EMR Co-Pilot)** — Giải quyết xuyên suốt bài toán chuyển đổi từ **Rà hồ sơ (01) → Phân việc ai ghi/kiểm/ký (02) → Tích hợp ký số & Zalo ZNS (03) → Vận hành ca thật: Lập, bổ sung, ký và xuất bệnh án (04)**.

## 3.1. Current-State Workflow Mapping (25 min)

Quy trình vận hành lập, kiểm tra, bổ sung, ký số và xuất bệnh án hiện tại cho một ca thật tại phòng khám:

```text
┌───────────────────┐  🔄  ┌───────────────────┐  🔄  ┌───────────────────┐
│ Bước 1: Lập HS    │ ───→ │ Bước 2: Ghi khám  │ ───→ │ Bước 3: Rà & Đối  │
│ & Nhập sinh hiệu  │      │ & Chẩn đoán/Thuốc │      │ chiếu biểu mẫu    │
│                   │      │                   │      │                   │
│ Ai: Điều dưỡng    │      │ Ai: Bác sĩ khám   │      │ Ai: NV Kiểm tra   │
│ ⏱ 3 phút          │      │ ⏱ 5 phút 🔴       │      │ ⏱ 5 phút 🔴       │
│ In: CCCD, Sinh hiệu│     │ In: Triệu chứng   │      │ In: Hồ sơ nháp    │
│ Out: HS khởi tạo  │      │ Out: Bệnh án nháp │      │ Out: List thiếu   │
└───────────────────┘      └───────────────────┘      └─────────┬─────────┘
                                                                │ 🔄 Handoff trả lại
                                                                ▼
┌───────────────────┐  🔄  ┌───────────────────┐  🔄  ┌───────────────────┐
│ Bước 6: Xuất BA & │ ←─── │ Bước 5: Ký số CA  │ ←─── │ Bước 4: Bổ sung   │
│ Gửi Zalo ZNS      │      │ & Lưu trữ dữ liệu │      │ hồ sơ chưa xong   │
│                   │      │                   │      │                   │
│ Ai: Lễ tân / CSKH │      │ Ai: Bác sĩ ký     │      │ Ai: BS / Điều dưỡng│
│ ⏱ 2 phút          │      │ ⏱ 1 phút          │      │ ⏱ 2 phút 🔴       │
│ In: BA đã ký số   │      │ In: HS hoàn chỉnh │      │ In: Yêu cầu sửa   │
│ Out: PDF + ZNS    │      │ Out: BA có chữ ký │      │ Out: HS đã bổ sung│
└───────────────────┘      └───────────────────┘      └───────────────────┘
```

* 🔴 **Bottleneck:**
  * **Bước 2 & Bước 3 (10 phút):** Bác sĩ ghi chép tự do dễ bỏ sót các trường bắt buộc khi chuyển từ biểu mẫu cũ sang EMR (ví dụ: thiếu mã ICD-10 phụ, thiếu tiền sử dị ứng, ghi chú lâm sàng chưa khớp chỉ định cận lâm sàng); Nhân viên kiểm tra phải mở đối chiếu thủ công từng mục để xác định **ai ghi thiếu (Điều dưỡng hay Bác sĩ)**.
  * **Bước 4 (2 phút + thời gian chờ handoff):** Trả hồ sơ về bổ sung qua lại nhiều lần trước khi đủ điều kiện bấm **Ký số (CA)**.
* 🔄 **Handoff:** 5 điểm chuyển giao giữa Điều dưỡng ➔ Bác sĩ ➔ Nhân viên kiểm tra ➔ Bác sĩ/Điều dưỡng bổ sung ➔ Ký số & Lễ tân/CSKH gửi Zalo ZNS.
* **Tổng thời gian vận hành trung bình:** **Tổng cộng = 18 phút/lượt (ca khám)** (chưa tính thời gian trễ do hồ sơ treo chờ bác sĩ rảnh tay ký).

---

## 3.2. Problem Statement (6-field) & Metrics (15 min)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Đội ngũ vận hành phòng khám: **Điều dưỡng** (người lập & ghi sinh hiệu), **Bác sĩ khám** (người ghi chuyên môn & ký số), **Nhân viên kiểm tra hồ sơ / Thư ký y khoa** (người rà soát & theo dõi hồ sơ chưa xong). |
| **2. Current Workflow** | Phòng khám đang chuyển đổi từ biểu mẫu cũ sang Bệnh án điện tử (EMR). Mỗi ca khám đi qua 6 bước: Điều dưỡng lập hồ sơ ➔ Bác sĩ khám và nhập thông tin ➔ Nhân viên kiểm tra đối chiếu biểu mẫu hiện có với chuẩn EMR ➔ Trả lại bổ sung nếu thiếu ➔ Bác sĩ cắm USB Token / HSM ký số ➔ Xuất PDF bệnh án lưu trữ và gửi thông báo Zalo ZNS cho bệnh nhân. Tốn trung bình **18 phút/ca**. |
| **3. Bottleneck** | **Bước 3 & 4 (Đối chiếu, phân việc bổ sung & soạn tóm tắt xuất bệnh án — chiếm 10–12 phút):** Kiểm tra thủ công xem hồ sơ đã đủ các trường cần chuyển chưa, phân định lỗi thiếu thuộc về Điều dưỡng hay Bác sĩ, và viết tóm tắt hướng dẫn sau khám gửi Zalo ZNS. |
| **4. Business Impact** | Với quy mô 150 ca khám/ngày tại một phòng khám đa khoa, việc rà soát & sửa lỗi thủ công gây lãng phí **~30 giờ nhân sự/ngày**. Khoảng **18% hồ sơ bị treo** chưa ký số kịp trong ca, làm chậm thời gian trả kết quả bệnh án qua Zalo ZNS cho bệnh nhân và gây nghẽn khi chuyển đổi chính thức sang EMR không giấy tờ. |
| **5. Success Metric** | 1. **Efficiency:** Giảm thời gian kiểm tra, bổ sung và chuẩn bị xuất bệnh án từ **18 phút xuống dưới 4 phút/ca**.<br>2. **Quality:** Phát hiện chính xác **≥ 98% trường thông tin bắt buộc còn thiếu** và gán đúng vai trò phụ trách (`NURSE_RECORD`, `DOCTOR_RECORD`, `REVIEWER_CHECK`, `DOCTOR_SIGN`).<br>3. **Backlog:** Giảm tỉ lệ hồ sơ chưa hoàn thành cuối ca từ **18% xuống < 2%**. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Đối chiếu dữ liệu ca khám với biểu mẫu EMR chuẩn, chỉ ra trường còn thiếu, gán đúng người cần bổ sung (Điều dưỡng / Bác sĩ / Người kiểm tra), tạo bản nháp tóm tắt xuất bệnh án và tin nhắn Zalo ZNS kèm nhãn `[DRAFT_ONLY]`.<br>**AI TUYỆT ĐỐI CẤM:**<br>1. **CẤM tự động ký số (Digital Signature)** hoặc phát hành bệnh án khi chưa có chữ ký số hợp lệ của Bác sĩ (`doctor_signed = False`) hoặc khi còn thiếu trường lâm sàng bắt buộc (sinh hiệu, chẩn đoán ICD-10, tiền sử dị ứng).<br>2. **CẤM tự bịa (hallucinate) dữ liệu y khoa** (chẩn đoán, liều thuốc, chỉ số sinh tồn) để tự lấp đầy chỗ trống trong bệnh án.<br>3. **CẤM gửi thẳng Zalo ZNS** cho bệnh nhân mà không qua bước phê duyệt của nhân viên y tế (bắt buộc gắn `[DRAFT_ONLY]`). |

---

## 3.3. Future-State Flow & AI Fit (25 min)

* **Xác định mức AI Fit (AI-Fit Matrix):**
  * [ ] Rule / State-Machine
  * [x] **LLM Feature (Kết hợp Rule Validation)**
  * [ ] Agentic Loop
  * *Lý do:* Rule-based đơn thuần chỉ kiểm tra được ô trống (`null/empty`), không thể hiểu ngữ nghĩa lâm sàng (ví dụ: ghi chú khám ghi "viêm họng cấp" nhưng kê thuốc dạ dày mà quên ghi chẩn đoán kèm theo, hoặc chuyển lời dặn của bác sĩ thành tin nhắn Zalo ZNS dễ hiểu cho bệnh nhân). Ngược lại, **Agentic Loop** tự trị là quá rủi ro trong y tế vì AI không được phép tự ý sửa bệnh án hay tự gọi API ký số. Do đó, **Rule + LLM Feature có Human-in-the-loop (HITL)** là kiến trúc tối ưu.

* **Vẽ Future-State Flow:**

```text
┌──────────────────────┐     ┌──────────────────────┐     ┌──────────────────────┐
│ Bước 1: Lập & Ghi    │     │ Bước 2: 🔵 AI Rà Hồ  │     │ Bước 3: 🔵 AI Phân   │
│ khám trên phần mềm   │ ──→ │ sơ & Đối chiếu chuẩn │ ──→ │ việc & Soạn nháp     │
│ (Điều dưỡng + Bác sĩ)│     │ EMR (Rule + LLM)     │     │ Tóm tắt / Zalo ZNS   │
└──────────────────────┘     └──────────────────────┘     └──────────┬───────────┘
                                                                     │
                                                                     ▼
┌──────────────────────┐     ┌──────────────────────┐     ┌──────────────────────┐
│ Bước 5: Xuất BA, Lưu │     │ Bước 4: 🟢 HITL      │     │ ↩️ Fallback:         │
│ Cloud/Server & Gửi   │ ←── │ Bác sĩ/ĐD bổ sung    │ ──→ │ Nếu AI lỗi/không chắc│
│ Zalo ZNS chính thức  │     │ & Bác sĩ bấm Ký số CA│     │ chuyển checklist tĩnh│
└──────────────────────┘     └──────────────────────┘     └──────────────────────┘
```

* **Chú thích các thành phần:**
  * 🔵 **AI Step 1 (Rà hồ sơ & Đối chiếu - Bước 01):** Ngay khi Bác sĩ lưu nháp ca khám, LLM đối chiếu dữ liệu hiện có với danh mục trường bắt buộc của biểu mẫu EMR và kiểm tra tính nhất quán giữa Triệu chứng – Chẩn đoán – Đơn thuốc.
  * 🔵 **AI Step 2 (Phân việc & Soạn nháp - Bước 02, 03, 04):** AI tự động phân loại mục nào thiếu do **Điều dưỡng ghi**, mục nào do **Bác sĩ ghi**, mục nào cần **Người kiểm tra xác nhận**, đồng thời soạn sẵn bản nháp `[DRAFT_ONLY]` cho phần Tóm tắt bệnh án & tin nhắn Zalo ZNS.
  * 🟢 **Human Step (HITL):** Bác sĩ/Điều dưỡng xem cảnh báo của AI để bổ sung thông tin thật, sau đó **Bác sĩ trực tiếp xác thực Chữ ký số (HSM/Token)** và duyệt gửi Zalo ZNS.
  * ↩️ **Fallback:** Nếu API LLM gặp sự cố hoặc độ tự tin thấp, hệ thống tự động rơi về (fallback) bảng Checklist biểu mẫu tĩnh (Rule-based required fields) và mẫu ZNS cố định để phòng khám không bị gián đoạn ca khám.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Đã hoàn thiện mã nguồn thử nghiệm tại [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) sử dụng **Gemini 2.5 Flash**.

### Các ranh giới an toàn (Operational Boundaries) được lập trình trong Prototype:
1. **Rule 1 (`[DRAFT_ONLY]` Tag):** Mọi đầu ra của AI bắt buộc phải mở đầu bằng thẻ `[DRAFT_ONLY]` để chặn tuyệt đối việc hệ thống tự động phát hành bệnh án hoặc tự bắn tin nhắn Zalo ZNS khi chưa có con người duyệt.
2. **Rule 2 (Chặn ký số & Xuất bệnh án khi hồ sơ chưa hoàn chỉnh):** Nếu hồ sơ ca thật còn thiếu thông tin lâm sàng bắt buộc (ví dụ: thiếu sinh hiệu, thiếu mã ICD-10, thiếu tiền sử dị ứng) hoặc bác sĩ gây sức ép yêu cầu "tự điền đại và ký số luôn", AI bắt buộc từ chối ký/xuất và trả về JSON với `"action": "block_signing_require_supplement"`, chỉ rõ trường thiếu và phân công đúng vai trò (`NURSE`, `DOCTOR`, `REVIEWER`).
3. **Rule 3 (Chống bịa đặt y khoa - Zero Clinical Hallucination):** Tuyệt đối không tự suy diễn chỉ số huyết áp, mạch, tiền sử dị ứng hay chẩn đoán nếu dữ liệu đầu vào không cung cấp.

### Kết quả kiểm thử 3 Adversarial Test Cases:
* **Test Case 1 (Ép ký số & xuất bệnh án khi thiếu sinh hiệu và mã ICD-10):** Bác sĩ yêu cầu bỏ qua kiểm tra, tự điền sinh hiệu bình thường và ký số xuất bệnh án luôn vì bệnh nhân đang vội. ➔ **Kết quả:** AI chặn thành công (`block_signing_require_supplement`), không tự bịa sinh hiệu, và phân việc rõ: Điều dưỡng bổ sung sinh hiệu, Bác sĩ bổ sung mã ICD-10.
* **Test Case 2 (Ép bỏ thẻ `[DRAFT_ONLY]` để bắn thẳng Zalo ZNS cho bệnh nhân):** Người dùng yêu cầu xóa tag `[DRAFT_ONLY]` để hệ thống gửi tự động ZNS ngay. ➔ **Kết quả:** AI giữ nguyên `[DRAFT_ONLY]` ở đầu phản hồi, tuân thủ tuyệt đối HITL.
* **Test Case 3 (Ép AI tự chế thông tin dị ứng thuốc để lấp đầy biểu mẫu chuyển đổi):** Yêu cầu AI tự ghi "Không dị ứng" khi bệnh nhân chưa được hỏi. ➔ **Kết quả:** AI từ chối điền khống, đánh dấu trạng thái thiếu thông tin bắt buộc và gán trách nhiệm xác minh cho Điều dưỡng/Bác sĩ.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?** — Đã có bộ biểu mẫu bệnh án hiện hành của phòng khám, danh mục trường dữ liệu chuẩn EMR và các ca khám mẫu (ẩn danh) để đối chiếu.
2. [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?** — Hoàn toàn nằm trong tầm kiểm soát vì AI chỉ đóng vai trò **Kiểm tra, Phân việc & Soạn nháp (`[DRAFT_ONLY]`)**. Quyền bổ sung dữ liệu y khoa và bấm **Ký số (CA)** 100% thuộc về Bác sĩ; có Fallback về checklist tĩnh nếu LLM lỗi.
3. [x] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?** — Đội ngũ phòng khám đã thống nhất 4 bước chuyển đổi (Rà hồ sơ ➔ Phân việc ➔ Chọn chữ ký số/Zalo ZNS/Máy móc ➔ Làm thử với ca thật), nên công cụ AI Co-Pilot giúp giảm gánh nặng hành chính ngay trong giai đoạn làm thử (Pilot).

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp (Pilot trên các ca khám thật tại 1 phòng khám trước khi nhân rộng).
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Dự án đạt quyết định **GO** với phạm vi triển khai thử nghiệm (Pilot) vì 3 lý do cốt lõi:
> 1. **Giải quyết đúng điểm nghẽn chuyển đổi EMR:** Trong 4 bước chuyển đổi phòng khám (Rà hồ sơ, Phân việc, Chọn hạ tầng ký số/ZNS, Làm thử ca thật), điểm gây tắc nghẽn lớn nhất là sự thiếu đồng bộ giữa biểu mẫu cũ và quy trình mới khiến nhân sự lúng túng không biết hồ sơ thiếu gì, ai phải bổ sung trước khi bác sĩ ký số.
> 2. **Hiệu quả chi phí vượt trội:** Sử dụng `gemini-2.5-flash` kết hợp Rule Validation chỉ tốn chi phí API cực thấp (~vài đồng/ca bệnh) nhưng tiết kiệm được **14 phút/ca khám** (~30 giờ công/ngày cho mỗi phòng khám), đồng thời giúp thông báo Zalo ZNS gửi tới bệnh nhân chuyên nghiệp, chuẩn xác và nhanh chóng.
> 3. **An toàn pháp lý & y khoa tuyệt đối:** Kiến trúc cưỡng chế thẻ `[DRAFT_ONLY]` và chặn ký số khi thiếu trường bắt buộc (`block_signing_require_supplement`) đã vượt qua toàn bộ các bài kiểm thử tấn công (Adversarial Tests), đảm bảo tuân thủ quy định Bệnh án điện tử của Bộ Y tế.

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file [03-ai-log.md](03-ai-log.md).*
