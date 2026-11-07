# 📄 01 — Problem Scan & Quick Assessment (Vin Smart Future)

> **Hạng mục nộp bài:** `01-problem-scan.md` (Phase 1: SCAN & Phase 2: QUICK-ASSESS)  
> **Đơn vị mục tiêu:** **Vinmec (Hệ thống Phòng khám & Chuỗi Chuyển đổi Bệnh án Điện tử EMR)** thuộc hệ sinh thái **Vin Smart Future (Vingroup)**.

---

## 🏛️ Bối cảnh khảo sát thực tế

Trong lộ trình chuyển đổi từ hồ sơ giấy/biểu mẫu rời rạc sang **Bệnh án điện tử (EMR)** chính thức tại hệ thống phòng khám, đội ngũ vận hành phải trải qua **4 giai đoạn cốt lõi**:
1. **01 — Rà hồ sơ đang dùng:** Đối chiếu bệnh án, biểu mẫu và dữ liệu hiện có với phần cần chuyển.
2. **02 — Phân việc trong phòng khám:** Thống nhất ai ghi, ai kiểm tra, ai ký và ai theo dõi hồ sơ chưa xong.
3. **03 — Chọn chữ ký số và máy móc:** Chọn nhà cung cấp chữ ký số (CA/HSM), Zalo ZNS, máy móc thiết bị và nơi lưu dữ liệu.
4. **04 — Làm thử với ca thật:** Cả phòng khám làm thử các bước lập, bổ sung, ký và xuất bệnh án trước khi dùng chính thức.

Qua khảo sát thực địa, cả 4 giai đoạn này đều xuất hiện những điểm nghẽn (bottlenecks) thủ công gây chậm tiến độ chuyển đổi và làm quá tải đội ngũ Bác sĩ, Điều dưỡng, Nhân viên Kế hoạch tổng hợp (KHTH).

---

## 🔍 Phase 1 — SCAN: Bảng quét cơ hội bằng 4 Lenses

| # | Công ty thành viên (Subsidiary) | Lens áp dụng | Mô tả ngắn bài toán & Điểm nghẽn thực tế |
|---|---|---|---|
| **01** | **Vinmec (Hệ thống Phòng khám)** | **Tốn thời gian (Time-consuming)** | **01. Rà hồ sơ đang dùng:** Chuyên viên quản trị dữ liệu và bác sĩ phải đối chiếu thủ công hàng trăm mẫu bệnh án, phiếu chỉ định và trường dữ liệu hiện có với cấu trúc Bệnh án điện tử (EMR) mới. Mất trung bình **25–30 phút/biểu mẫu**, dễ bỏ sót các trường bắt buộc theo chuẩn Bộ Y tế hoặc lệch kiểu dữ liệu. |
| **02** | **Vinmec (Hệ thống Phòng khám)** | **Lặp lại (Repetitive)** | **02. Phân việc trong phòng khám:** Mỗi ngày có hàng trăm hồ sơ đi qua nhiều người nhưng chưa thống nhất luồng kiểm soát tự động cho việc *ai ghi (Điều dưỡng/Bác sĩ), ai kiểm tra (KHTH/Thư ký y khoa), ai ký số và ai theo dõi hồ sơ chưa xong*. Nhân viên kiểm tra phải mở từng hồ sơ nhắc thủ công, dẫn đến **15–20% hồ sơ bị treo** cuối ngày. |
| **03** | **Vinmec (Hệ thống Phòng khám)** | **AI có thể tốt hơn (AI-upgrade)** | **03. Chọn chữ ký số, Zalo ZNS và máy móc:** Quá trình chuẩn hóa tích hợp giữa nhà cung cấp chữ ký số (USB Token/HSM/Remote Signing), hệ thống gửi thông báo **Zalo ZNS** cho bệnh nhân, cấu hình máy móc và nơi lưu dữ liệu còn thủ công; các mẫu thông báo ZNS gửi bệnh nhân sau khi ký xuất bệnh án còn rập khuôn, chưa cá nhân hóa theo dặn dò của bác sĩ. |
| **04** | **Vinmec (Hệ thống Phòng khám)** | **Pain từ người khác (Stakeholder Pain)** | **04. Làm thử với ca thật (Lập, bổ sung, ký và xuất bệnh án):** Khi toàn phòng khám chạy thử với ca bệnh thật, quy trình *Lập ➔ Bổ sung ➔ Kiểm tra ➔ Ký số ➔ Xuất bệnh án* bị nghẽn do hồ sơ thiếu trường thông tin phải trả đi trả lại giữa Bác sĩ và Điều dưỡng (mất **15–20 phút/ca**), gây ức chế cho y bác sĩ và khiến bệnh nhân chờ lâu. |
| **05** | **Vinhomes / Xanh SM** | **Lặp lại (Repetitive)** | **05. Phân loại & điều phối yêu cầu hỗ trợ kỹ thuật đa kênh:** Nhân viên CSKH phải đọc, trích xuất thông tin và gán nhãn thủ công hàng ngàn ticket phản ánh mỗi ngày trước khi chuyển sang bộ phận kỹ thuật xử lý. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Từ bảng SCAN trên, nhóm chọn **3 bài toán sát sườn nhất** trong chuỗi chuyển đổi Bệnh án điện tử tại phòng khám (**#01 Rà hồ sơ đang dùng**, **#02 Phân việc & theo dõi hồ sơ chưa xong**, và **#04 Làm thử với ca thật: lập, bổ sung, ký và xuất bệnh án**) để đánh giá nhanh.

---

### 🗂️ Quick Problem Card #01 — Rà hồ sơ đang dùng & Đối chiếu chuẩn EMR

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #01                                      │
│                                                             │
│ Bài toán (1 câu): Đối chiếu bệnh án, biểu mẫu và dữ liệu    │
│ hiện có của phòng khám với chuẩn dữ liệu EMR cần chuyển.    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec (Hệ thống Phòng khám)        │
│                                                             │
│ Ai đang đau (Actor)? Chuyên viên KHTH / Quản trị dữ liệu,   │
│ Bác sĩ trưởng phòng khám và Đội triển khai phần mềm EMR.    │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Tập hợp toàn bộ mẫu bệnh án, biểu mẫu giấy/Word hiện có│
│   ──> 2. Đọc và bóc tách từng mục/trường dữ liệu ra Excel   │
│   ──> 3. So khớp từng mục với schema dữ liệu chuẩn của EMR  │
│   ──> 4. Lập danh sách mục thiếu/thừa để sửa biểu mẫu đích  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 25 phút/mẫu) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (LLM đọc biểu mẫu hiện tại, tự động đối chiếu trường tương  │
│ ứng sang chuẩn EMR và gắn cờ các trường còn thiếu/lệch)     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm thời gian rà soát đối chiếu từ 25 min ──> 4 min/mẫu;│
│    phát hiện đúng >= 95% trường dữ liệu chênh lệch."        │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

### 🗂️ Quick Problem Card #02 — Phân việc trong phòng khám & Theo dõi hồ sơ chưa xong

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #02                                      │
│                                                             │
│ Bài toán (1 câu): Thống nhất và tự động hóa kiểm soát ai    │
│ ghi, ai kiểm tra, ai ký và ai theo dõi hồ sơ chưa xong.     │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec (Hệ thống Phòng khám)        │
│                                                             │
│ Ai đang đau (Actor)? Điều dưỡng (người ghi đầu vào), Bác sĩ │
│ (người ghi khám & ký), Nhân viên kiểm tra & Quản lý PK.     │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Điều dưỡng nhập hành chính/sinh hiệu vào hồ sơ         │
│   ──> 2. Bác sĩ khám, nhập bệnh sử, chẩn đoán, chỉ định     │
│   ──> 3. Người kiểm tra mở từng hồ sơ rà lỗi thiếu/chưa ký  │
│   ──> 4. Nhắn tin nhắc riêng từng người bổ sung cuối ca     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10 phút/ca)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4            │
│ (AI quét nội dung hồ sơ, phát hiện mục chưa hoàn chỉnh và   │
│ gán đúng vai trò phải xử lý: Người ghi / Người kiểm / Ký)   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm tỉ lệ hồ sơ treo chưa xong cuối ngày từ 18% ──>     │
│    dưới 2%; giảm thời gian rà phân việc từ 10 min ──> 1 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

### 🗂️ Quick Problem Card #03 — Làm thử với ca thật: Lập, bổ sung, ký số và xuất bệnh án

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #03 (Giai đoạn 04 kết hợp 01, 02, 03)    │
│                                                             │
│ Bài toán (1 câu): Hỗ trợ cả phòng khám làm thử trơn tru các │
│ bước lập, bổ sung, kiểm tra, ký số và xuất bệnh án (kèm     │
│ thông báo Zalo ZNS) trên ca bệnh thật trước khi dùng chính  │
│ thức.                                                       │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec (Hệ thống Phòng khám)        │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ khám, Điều dưỡng, Nhân viên     │
│ kiểm tra hồ sơ, Lễ tân xuất bệnh án & Bệnh nhân.            │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Điều dưỡng lập bệnh án, ghi nhận sinh hiệu ca thật     │
│   ──> 2. Bác sĩ ghi khám lâm sàng, chẩn đoán & hướng xử trí │
│   ──> 3. NV kiểm tra đối chiếu biểu mẫu, trả về bổ sung     │
│   ──> 4. Bác sĩ ký số (CA/HSM) & lưu trữ dữ liệu bệnh án    │
│   ──> 5. Xuất bệnh án & soạn thông báo Zalo ZNS gửi BN      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2, 3 & 5 (⏱ 14 phút)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 & 5         │
│ (AI tự động đối chiếu trường thiếu theo biểu mẫu mới, phân  │
│ đúng người bổ sung, kiểm tra điều kiện an toàn trước khi    │
│ ký số, và soạn nháp tóm tắt xuất bệnh án / Zalo ZNS)        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm tổng thời gian hoàn tất & xuất bệnh án ca thật từ   │
│    18 min ──> dưới 4 min; 100% hồ sơ đủ chuẩn trước khi ký" │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Quyết định lựa chọn bài toán đi tiếp vào Deep-Dive (Phase 3)

Nhóm quyết định chọn **Card #03 (Làm thử với ca thật: Lập, bổ sung, kiểm tra, ký số và xuất bệnh án — tích hợp xuyên suốt 4 bước 01, 02, 03, 04)** làm bài toán trọng tâm cho báo cáo [02-deep-dive-report.md](02-deep-dive-report.md) vì:
1. **Tính tổng thể & Thực tiễn cao:** Giai đoạn **"04. Làm thử với ca thật"** chính là nơi hội tụ kết quả của **01 (Rà hồ sơ đang dùng)**, **02 (Phân việc ai ghi, ai kiểm tra, ai ký, ai theo dõi)** và **03 (Hạ tầng chữ ký số, Zalo ZNS, lưu trữ dữ liệu)**.
2. **Đo lường trực tiếp theo thời gian thực (Real-time Impact):** Giải quyết trực tiếp nỗi đau của cả phòng khám ngay tại bàn khám, rút ngắn thời gian hoàn tất bệnh án và trả kết quả qua Zalo ZNS cho bệnh nhân mà vẫn đảm bảo an toàn pháp lý của chữ ký số.

