# 02 — Deep-Dive Report: Phân loại và chuyển tiếp phản ánh cư dân Vinhomes

**Lab:** AI Product Scoping — Vin Smart Future  
**Use case được đánh giá:** Quick Problem Card #2 trong [01-problem-scan.md](./01-problem-scan.md)  
**Phạm vi:** Phase 3 — DEEP-DIVE và Phase 5 — EVALUATE  
**Trạng thái thông tin:** Scoping sơ bộ; chưa có dữ liệu vận hành hoặc xác nhận từ stakeholder. Các mốc thời gian bên dưới được dùng làm **giả định scoping đã thống nhất cho bài làm**, chưa phải baseline đo tại Vinhomes.

## 1. Tóm tắt điều hành

Báo cáo đánh giá khả năng hỗ trợ nhân viên CSKH phân loại phản ánh cư dân và chuyển ticket đến đúng bộ phận. Phạm vi đề xuất chỉ gồm **đề xuất chủ đề, mức độ ưu tiên và bộ phận nhận ticket**; AI không xử lý sự cố, đóng ticket hay tự gửi cam kết cho cư dân.

Giả thuyết cần kiểm chứng là việc đọc nội dung tự do và chọn tuyến xử lý có thể tạo thêm thời gian chờ hoặc khiến ticket bị chuyển lại. Chưa có số liệu về khối lượng ticket, thời gian xử lý, tỷ lệ chuyển sai, danh mục phân loại hay yêu cầu tích hợp. Vì vậy, kết luận hiện tại là **NOT YET**: chưa nên triển khai AI trước khi xác nhận quy trình, dữ liệu, baseline và mức chấp nhận của nhân viên. Nếu các điều kiện được đáp ứng, nên bắt đầu bằng pilot hẹp có nhân viên duyệt mọi đề xuất.

## 2. Current-State Workflow Mapping

Sơ đồ hiện trạng tương ứng với [04-workflow-diagram.png](./04-workflow-diagram.png). Quy trình dưới đây là mô tả scoping dựa trên Quick Problem Card. Các mốc thời gian đã thống nhất để dùng trong bài; cần walkthrough và time study với CSKH/bộ phận nhận ticket để xác nhận trước khi xem là baseline vận hành.

| Bước | Người thực hiện | Hoạt động và đầu vào/đầu ra | Handoff | Thời gian thao tác |
|---|---|---|---|---:|
| 1. Gửi phản ánh | Cư dân | Nhập nội dung và gửi phản ánh qua ứng dụng; đầu ra là ticket/nội dung chờ tiếp nhận. | Ứng dụng → CSKH | 1–3 phút của cư dân; không tính vào effort nhân viên |
| 2. Tiếp nhận và phân loại | CSKH | 🔴 Đọc nội dung, xác định chủ đề, mức độ ưu tiên và thông tin còn thiếu. | Có thể cần hỏi lại cư dân hoặc tham khảo quy trình nội bộ. | 2–4 phút/ticket |
| 3. Chọn tuyến và chuyển ticket | CSKH | 🔴 Chọn ban quản lý/bộ phận phụ trách và chuyển ticket. Nếu chọn sai, ticket có thể phải chuyển lại. | CSKH → bộ phận nhận xử lý | 1–3 phút/ticket |
| 4. Xác nhận tiếp nhận | Bộ phận nhận ticket | Mở ticket, xác nhận bộ phận có thể tiếp nhận hoặc báo cần chuyển tuyến. | Bộ phận xử lý → CSKH nếu cần cập nhật | 2–5 phút thao tác; thời gian chờ trong hàng đợi chưa xác định |
| 5. Xử lý sự cố | Bộ phận chuyên môn | Khảo sát và khắc phục vấn đề theo quy trình chuyên môn sau khi nhận ticket. | Bộ phận chuyên môn ↔ cư dân/CSKH tùy trường hợp | Chưa xác định; phụ thuộc loại sự cố |
| 6. Theo dõi và phản hồi | CSKH | Theo dõi trạng thái xử lý, liên hệ bộ phận phụ trách khi cần và phản hồi cư dân theo quy trình. | Bộ phận xử lý → CSKH → cư dân | 1–3 phút/lần cập nhật; số lần cập nhật chưa biết |

**Tổng effort thao tác đến lúc bộ phận xác nhận tiếp nhận:** khoảng **5–12 phút nội bộ** (CSKH 3–7 phút + bộ phận nhận ticket 2–5 phút), không gồm thời gian cư dân gửi phản ánh, thời gian chờ trong hàng đợi, thời gian xử lý sự cố và các lần cập nhật sau đó. Nếu tính cả thời gian nhập phản ánh của cư dân, tổng thao tác của các bên là khoảng **6–15 phút**. Effort này không bao gồm bước 5–6; thời gian chờ và tổng elapsed time đầu-cuối chưa xác định.

### Bottleneck và Business Impact

- **Bottleneck giả thuyết:** bước 2–3, nơi CSKH diễn giải nội dung tự do và chọn đúng tuyến xử lý.
- **Hậu quả có thể xảy ra:** phân loại không thống nhất, chuyển ticket sai hoặc thiếu thông tin, phát sinh chuyển vòng và kéo dài thời gian đến khi đúng bộ phận nhận việc.
- **Kịch bản định lượng minh họa (toàn bộ đầu vào là giả định, không phải dữ liệu Vinhomes):** nếu một nhóm xử lý **100 ticket/ngày**, với thời gian CSKH phân loại và chuyển tuyến trung bình **5 phút/ticket**, effort hiện trạng cho tác vụ này là `100 × 5 ÷ 60 = 8,3 giờ công/ngày`. Nếu pilot đạt mục tiêu **3 phút/ticket**, effort là `100 × 3 ÷ 60 = 5 giờ công/ngày`; tiết kiệm tiềm năng là `8,3 − 5 = 3,3 giờ công/ngày`. Với giả định hoạt động **5 ngày/tuần**, mức tiết kiệm tương ứng khoảng **16,7 giờ công/tuần**.
- **Diễn giải thận trọng:** các con số trên là kịch bản tính toán để minh họa quy mô tiềm năng, không phải số ticket, baseline hoặc kết quả pilot đã đo tại Vinhomes. Chưa tính số ticket chuyển sai, thời gian chờ, chi phí nhân công, chi phí triển khai hoặc hiệu quả thực tế của AI; do đó chưa thể kết luận ROI hay khoản tiết kiệm tiền.

## 3. Problem Statement (6-field)

| Field | Problem statement |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH tiếp nhận và phân loại phản ánh; ban quản lý hoặc bộ phận chuyên môn nhận ticket và xử lý; cư dân gửi phản ánh và nhận cập nhật. Vai trò thực tế cần được xác nhận tại từng cơ sở. |
| **2. Current Workflow** | Theo mô hình scoping: (1) cư dân gửi phản ánh qua ứng dụng; (2) CSKH đọc và phân loại; (3) CSKH chọn bộ phận và chuyển ticket; (4) bộ phận nhận xác nhận tiếp nhận; (5) bộ phận chuyên môn xử lý; (6) CSKH theo dõi và phản hồi cư dân. Công cụ, hệ thống ticket và các nhánh ngoại lệ cần được xác minh với stakeholder. |
| **3. Bottleneck** | Việc diễn giải nội dung tự do, nhận diện nhiều vấn đề trong một ticket và đối chiếu với danh mục/bộ phận có thể làm chậm phân loại hoặc dẫn đến chuyển tuyến sai. Đây là giả thuyết, cần kiểm tra bằng dữ liệu ticket và phỏng vấn nhân viên. |
| **4. Business Impact** | Kịch bản minh họa với các đầu vào giả định (không phải dữ liệu Vinhomes): **100 ticket/ngày × 5 phút phân loại/chuyển tuyến ÷ 60 = 8,3 giờ công/ngày**. Nếu pilot đạt **3 phút/ticket**, effort còn **5 giờ/ngày**, tiết kiệm tiềm năng **3,3 giờ/ngày** hoặc **16,7 giờ/tuần** với giả định 5 ngày hoạt động. Chưa gồm chuyển sai, thời gian chờ, chi phí nhân công hay triển khai; chưa đủ dữ liệu để kết luận ROI. |
| **5. Success Metric** | Pilot cần đo (1) độ chính xác chủ đề và định tuyến trên tập ticket được nhân viên gán nhãn, (2) thời gian thao tác trung bình và trung vị từ lúc CSKH mở ticket đến lúc chuyển tuyến, (3) tỷ lệ ticket bị chuyển lại và (4) tỷ lệ đề xuất bị nhân viên sửa. Mốc thời gian scoping đã thống nhất là **3–7 phút/ticket** cho phân loại và chuyển tuyến. Mục tiêu pilot đề xuất: **≥90%** định tuyến đúng và thời gian thao tác trung bình **≤3 phút/ticket**; xác nhận target sau khi đo baseline thực tế. Không giảm độ chính xác/khả năng xử lý nhóm khẩn cấp để đổi lấy tốc độ. |
| **6. Operational Boundary** | AI chỉ được đề xuất nhãn chủ đề, mức ưu tiên, bộ phận nhận và nêu thông tin còn thiếu dựa trên ticket cùng danh mục được phê duyệt. CSKH duyệt/chỉnh sửa trước khi chuyển. AI không tự đóng ticket, tự cam kết thời hạn/chi phí, tự trả lời cư dân, hoặc tự quyết định xử lý chuyên môn. Ticket khẩn cấp, thiếu dữ liệu, ngoài danh mục hoặc độ tin cậy thấp phải chuyển về quy trình thủ công hiện hành. Chỉ dùng dữ liệu được phép truy cập; dữ liệu thử nghiệm phải được ẩn danh/phê duyệt theo chính sách áp dụng. |

## 4. AI Fit và Future-State Flow

### So sánh phương án

| Phương án | Phù hợp với tác vụ nào? | Hạn chế / rủi ro | Đánh giá |
|---|---|---|---|
| **Rule / state machine** | Kiểm tra trường bắt buộc, ánh xạ từ danh mục ticket đã chuẩn hóa, áp dụng tuyến xử lý cố định và quy tắc chuyển trường hợp khẩn cấp. | Khó bao phủ cách diễn đạt đa dạng, lỗi chính tả, ngữ cảnh hoặc nhiều vấn đề trong cùng nội dung nếu chỉ dựa vào từ khóa. | Nên dùng cho điều kiện rõ ràng, có tính quyết định và yêu cầu kiểm soát cao. |
| **LLM feature** | Đề xuất chủ đề, tóm tắt nội dung, phát hiện thông tin còn thiếu và gợi ý tuyến xử lý từ văn bản tự do. | Có thể phân loại sai hoặc tạo suy diễn; cần đánh giá trên dữ liệu đại diện, giới hạn đầu ra và có bước duyệt người. | Có thể thử nghiệm như tính năng hỗ trợ CSKH, không tự động thực thi hành động. |
| **Agentic loop** | Tự thực hiện nhiều bước qua các công cụ/hệ thống, ví dụ lấy thêm dữ liệu rồi tự chuyển tuyến. | Tăng quyền hành động, độ phức tạp kiểm soát và rủi ro gửi/chuyển nhầm; chưa có nhu cầu đã xác minh cho tự trị nhiều bước. | Chưa phù hợp ở giai đoạn này; không khuyến nghị cho pilot đầu tiên. |

**AI Fit đề xuất:** thử nghiệm **Rule + LLM feature có Human-in-the-loop**. Rule giữ các điều kiện bắt buộc và tuyến đã biết; LLM chỉ xử lý/đề xuất cách hiểu nội dung tự do. Nếu baseline cho thấy rule-based đạt độ chính xác và tốc độ yêu cầu, không cần bổ sung LLM.

### Future-State Flow đề xuất

```text
Cư dân gửi phản ánh
        ↓
Hệ thống tiếp nhận ticket và kiểm tra trường bắt buộc bằng rule
        ↓
Ticket thuộc nhóm khẩn cấp hoặc có điều kiện định tuyến rõ?
   ├─ Có → Chuyển theo quy trình ưu tiên đã được phê duyệt
   └─ Không / nội dung tự do
        ↓
🔵 AI đề xuất chủ đề, mức ưu tiên, bộ phận nhận và thông tin còn thiếu
        ↓
🟢 CSKH xem nội dung gốc, kiểm tra và duyệt/chỉnh sửa đề xuất
   ├─ Đạt yêu cầu → CSKH xác nhận chuyển ticket
   └─ Thiếu dữ liệu / không chắc chắn / ngoài danh mục
        → ↩️ Fallback: CSKH phân loại thủ công theo quy trình hiện tại
        ↓
Bộ phận phụ trách tiếp nhận và xử lý
        ↓
CSKH theo dõi trạng thái và phản hồi cư dân theo quy trình hiện hành
```

**Fallback:** Khi dịch vụ AI lỗi, không trả về cấu trúc hợp lệ, độ tin cậy dưới ngưỡng đã đánh giá hoặc gặp nội dung ngoài danh mục, ticket vẫn được tiếp nhận và xử lý thủ công. Không được để lỗi AI làm mất ticket hoặc trì hoãn luồng khẩn cấp.

## 5. Phase 5 — EVALUATE

### AI Readiness Checklist

| Câu hỏi readiness | Trạng thái | Căn cứ / việc cần làm |
|---|---|---|
| Có dữ liệu mẫu/logs sạch để test không? | **Chưa xác nhận** | Chưa có ticket mẫu, nhãn chuẩn, danh mục tuyến hoặc quyền truy cập được xác nhận. Cần xin dữ liệu phù hợp, ẩn danh và tạo tập đánh giá do nhân viên gán nhãn. |
| Rủi ro khi AI sai có được kiểm soát bằng HITL/fallback không? | **Có thể thiết kế được; cần kiểm thử** | Phạm vi đề xuất không cho AI tự chuyển/đóng ticket; nhân viên duyệt và quy trình thủ công là fallback. Cần thử lỗi phân loại, ticket khẩn cấp, nội dung đa vấn đề và sự cố dịch vụ AI. |
| Stakeholder sẵn sàng thay đổi quy trình không? | **Chưa xác nhận** | Chưa phỏng vấn CSKH, ban quản lý hoặc bộ phận nhận ticket. Cần xác nhận người chịu trách nhiệm duyệt, khả năng tích hợp và quy trình xử lý ngoại lệ. |

### Quyết định: **NOT YET**

Chưa đủ bằng chứng để khuyến nghị bắt đầu triển khai AI: workflow hiện mới là giả thuyết; chưa có baseline về thời gian, tỷ lệ định tuyến đúng hoặc chuyển ticket lại; dữ liệu và sự sẵn sàng của stakeholder cũng chưa được xác nhận. Vì vậy, chưa thể chứng minh LLM tốt hơn quy trình hiện tại hoặc rule-based.

**Các bước cần hoàn tất trước quyết định GO:**
1. Walkthrough quy trình thực tế với CSKH và ít nhất một bộ phận nhận ticket; xác nhận hệ thống, vai trò, tuyến xử lý và nhánh khẩn cấp.
2. Đo baseline trên mẫu ticket đại diện: thời gian phân loại, độ chính xác định tuyến, tỷ lệ chuyển lại và phân bố theo loại sự cố.
3. Xin phê duyệt sử dụng dữ liệu; ẩn danh ticket; xây bộ nhãn chuẩn và kiểm tra chất lượng nhãn.
4. So sánh quy trình hiện tại, rule-based và rule + LLM trên cùng tập đánh giá; đặt ngưỡng riêng cho nhóm khẩn cấp.
5. Nếu đạt tiêu chí và stakeholder đồng thuận, chạy pilot giới hạn với đề xuất AI ở chế độ hỗ trợ, bắt buộc CSKH duyệt và có cơ chế quay về xử lý thủ công.

## 6. Giới hạn và phạm vi kết luận

Báo cáo này đánh giá một cơ hội ở mức scoping, không khẳng định đây là quy trình thực tế đã được xác nhận của Vinhomes. Các khoảng thời gian trong báo cáo là giả định scoping đã thống nhất cho bài làm, không phải số đo hoặc baseline vận hành tại Vinhomes. Kết luận **NOT YET** có thể được xem xét lại sau khi hoàn tất xác minh quy trình, đo baseline, xác nhận dữ liệu, yêu cầu bảo vệ dữ liệu và khả năng vận hành pilot.
