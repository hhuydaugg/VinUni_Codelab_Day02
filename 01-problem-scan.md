# 01 — Problem Scan & Quick Assessment

**Lab:** AI Product Scoping — Vin Smart Future  
**Phạm vi:** Phase 1 — SCAN; Phase 2 — QUICK-ASSESS  
**Vai trò:** AI Product Engineer

## Bối cảnh và mục tiêu

Trong vai trò AI Product Engineer tại Vin Smart Future, tôi rà soát một số quy trình vận hành tại các công ty thành viên Vingroup để xác định nơi nhân viên hoặc khách hàng gặp trở ngại, sau đó đánh giá nhanh liệu AI có thể hỗ trợ hay không. Mục tiêu không phải đưa AI vào mọi quy trình, mà là chọn một vấn đề có người dùng và quy trình đủ rõ, có thể đo hiệu quả, và có ranh giới vận hành phù hợp.

Các ý tưởng dưới đây là giả thuyết ban đầu để scoping, không phải kết quả khảo sát nội bộ. Tôi chưa có quyền truy cập dữ liệu vận hành hoặc phỏng vấn nhân viên. Những khoảng thời gian ghi trong tài liệu là **ước tính giả thuyết để lập kế hoạch**, không phải số đo thực tế; cần kiểm chứng bằng quan sát, phỏng vấn và baseline trước khi dùng để kết luận hiệu quả.

## Phase 1 — SCAN

Tôi sử dụng bốn lenses trong worksheet — **Lặp lại (Repetitive), Tốn thời gian (Time-consuming), AI có thể tốt hơn (AI-upgrade), và Pain từ người khác (Stakeholder Pain)** — để lập danh sách cơ hội ban đầu.

| # | Công ty thành viên | Lens | Bài toán vận hành và giả thuyết bottleneck | Thời lượng thao tác giả thuyết/lượt (chưa đo) |
|---|---|---|---|---:|
| 1 | Xanh SM | Lặp lại | Khi khách hủy hoặc thay đổi điểm đến, điều phối viên cần kiểm tra lại thông tin chuyến và phân bổ cuốc. Cần xác minh tần suất thao tác và liệu các trường hợp phổ biến có thể giải quyết bằng quy tắc. | 2–5 phút/cuốc cần xử lý lại |
| 2 | Vinpearl | Pain từ người khác | Review về chất lượng phòng hoặc dịch vụ có thể cần được chú ý nhanh, nhưng quản lý phải rà soát nhiều kênh và xác định đội phụ trách. Cần xác minh cách review hiện được thu thập và xử lý. | 2–5 phút/review cần chuyển |
| 3 | VinFast | Lặp lại | Nhân viên đối chiếu hóa đơn sạc từ đối tác với dữ liệu hệ thống định kỳ. Khác biệt định dạng hoặc dữ liệu thiếu có thể làm tăng thao tác kiểm tra ngoại lệ. | 3–8 phút/hóa đơn cần đối chiếu |
| 4 | Vinhomes | AI-upgrade | Nhân viên CSKH đọc phản ánh cư dân, xác định nội dung và chuyển ticket đến đúng bộ phận. Nội dung tự do hoặc nhiều vấn đề trong một phản ánh có thể làm chậm phân loại và gây chuyển nhầm. | 3–7 phút/ticket để phân loại và chuyển |
| 5 | Vinmec | Pain từ người khác | Bác sĩ tổng hợp thông tin từ hồ sơ bệnh án để soạn tóm tắt xuất viện. Việc đọc nhiều phần hồ sơ và viết lại nội dung có thể tốn thời gian; độ chính xác lâm sàng là yêu cầu bắt buộc. | 20–40 phút/bản tóm tắt |
| 6 | Vinpearl | Tốn thời gian | Nhân viên đặt phòng đọc email đặt phòng đoàn, kiểm tra thông tin và tình trạng phòng, rồi chuẩn bị yêu cầu xử lý. Email thiếu hoặc không thống nhất thông tin có thể dẫn đến trao đổi bổ sung. | 15–30 phút/yêu cầu đặt phòng đoàn |

> Các khoảng thời gian trong bảng là giả thuyết scoping dựa trên việc phân rã tác vụ, không phải số liệu của doanh nghiệp hay kết quả quan sát. Chúng chỉ dùng để hình thành câu hỏi khảo sát ban đầu.

## Phase 2 — QUICK-ASSESS

Từ danh sách trên, tôi chọn ba vấn đề để đánh giá nhanh: **#2 Vinpearl**, **#4 Vinhomes** và **#5 Vinmec**. Các card tập trung vào tác vụ có phạm vi hẹp; thời gian thực tế, khối lượng và chất lượng hiện tại cần được đo trước khi quyết định triển khai.

### Quick Problem Card #1 — Vinpearl: Nhận diện và chuyển tiếp review cần chú ý

| Trường | Đánh giá |
|---|---|
| **Bài toán** | Quản lý khách sạn cần nhận diện review phản ánh vấn đề dịch vụ cần chú ý và chuyển đến đúng đội phụ trách. |
| **Công ty thành viên** | Vinpearl |
| **Actor / người chịu ảnh hưởng** | Quản lý khách sạn và các đội dịch vụ nhận xử lý; khách hàng có thể bị ảnh hưởng nếu phản ánh không được xem xét kịp thời. |
| **Workflow hiện tại (giả thuyết)** | 1. Nhân viên mở kênh review (1–2 phút/lượt rà soát). 2. Đọc và xác định nội dung cần chú ý (1–2 phút/review). 3. Chuyển review đến đội phụ trách (1–2 phút/review). 4. Theo dõi phản hồi hoặc việc xử lý (1–3 phút/lần cập nhật). |
| **Bottleneck cần xác minh** | Rà soát nhiều kênh, nhận diện mức độ cần chú ý và tìm đội phụ trách. Giả thuyết thao tác ban đầu ở bước đọc và chuyển là 2–4 phút/review, chưa gồm thời gian chờ phản hồi; cần kiểm tra bằng mẫu review và quan sát quy trình. |
| **AI có thể hỗ trợ** | Gom nhóm và đề xuất chủ đề, mức độ ưu tiên, đội phụ trách. Nhân viên xác nhận trước khi chuyển; phản hồi công khai vẫn do nhân viên soạn và duyệt. |
| **Metric thử nghiệm** | Mục tiêu đề xuất: ít nhất **90%** review thuộc nhóm ưu tiên được chuyển đúng đội trong **dưới 5 phút** kể từ khi được ghi nhận. Cần định nghĩa nhóm ưu tiên, đo baseline và xác nhận target với quản lý khách sạn. |
| **Quick Architecture** | LLM phân loại nội dung kết hợp danh mục/luật định tuyến và bước nhân viên duyệt. |

### Quick Problem Card #2 — Vinhomes: Phân loại và chuyển tiếp phản ánh cư dân

| Trường | Đánh giá |
|---|---|
| **Bài toán** | Nhân viên CSKH cần phân loại phản ánh cư dân gửi qua ứng dụng và chuyển ticket đến đúng bộ phận hoặc ban quản lý. |
| **Công ty thành viên** | Vinhomes |
| **Actor / người chịu ảnh hưởng** | Nhân viên CSKH tiếp nhận; ban quản lý hoặc đội kỹ thuật xử lý; cư dân chờ phản hồi và khắc phục vấn đề. |
| **Workflow hiện tại (giả thuyết)** | 1. Cư dân nhập và gửi phản ánh (1–3 phút của cư dân). 2. CSKH đọc nội dung, xác định chủ đề/mức ưu tiên (2–4 phút). 3. CSKH chọn bộ phận và chuyển ticket (1–3 phút). 4. Bộ phận xác nhận tiếp nhận (2–5 phút thao tác); thời gian ticket nằm trong hàng đợi chưa xác định. 5. CSKH theo dõi và phản hồi (1–3 phút cho mỗi lần cập nhật, không nhất thiết phát sinh ở mọi ticket). Tổng thao tác CSKH trước khi chuyển: 3–7 phút/ticket; tổng thao tác nội bộ đến xác nhận tiếp nhận: 5–12 phút, không gồm thời gian chờ. |
| **Bottleneck cần xác minh** | Phân loại nội dung tự do và chọn đúng bộ phận, nhất là khi phản ánh không rõ hoặc gồm nhiều vấn đề. Thao tác của CSKH ở bước phân loại và chuyển là 3–7 phút/ticket; chọn sai có thể gây chuyển vòng. Các thời lượng này là giả thuyết độ tin cậy thấp, chưa có baseline hay tỷ lệ chuyển sai đo được. |
| **AI có thể hỗ trợ** | Đề xuất chủ đề, mức độ ưu tiên và bộ phận nhận ticket; giữ nguyên nội dung cư dân cung cấp và nêu rõ trường hợp thiếu thông tin hoặc độ tin cậy thấp để CSKH xem xét. |
| **Metric thử nghiệm** | Giả thuyết baseline thao tác CSKH: **3–7 phút/ticket** cho phân loại và chuyển tuyến. Mục tiêu pilot đề xuất: **≥90%** ticket được phân loại/định tuyến đúng trên tập đánh giá do nhân viên gán nhãn; thời gian trung vị từ lúc CSKH mở ticket đến lúc chuyển tuyến **≤3 phút/ticket**. Đo baseline thực tế trước pilot và thống nhất cách tính “đúng” với các bộ phận nhận ticket. |
| **Quick Architecture** | **Rule + LLM hybrid:** rule xử lý danh mục và điều kiện rõ ràng; LLM đề xuất phân loại nội dung tự do; CSKH xác nhận trước khi chuyển. |
| **Ranh giới ban đầu** | Không tự động đóng ticket, tự gửi câu trả lời/cam kết cho cư dân, hoặc tự quyết định xử lý sự cố. Ticket khẩn cấp, thiếu dữ liệu hoặc không chắc chắn được chuyển cho nhân viên theo quy trình hiện hành. |

### Quick Problem Card #3 — Vinmec: Soạn nháp tóm tắt hồ sơ xuất viện

| Trường | Đánh giá |
|---|---|
| **Bài toán** | Bác sĩ cần tổng hợp thông tin trong hồ sơ bệnh án để soạn tóm tắt xuất viện đầy đủ, dễ hiểu và chính xác. |
| **Công ty thành viên** | Vinmec |
| **Actor / người chịu ảnh hưởng** | Bác sĩ soạn và duyệt tài liệu; bệnh nhân nhận bản tóm tắt. |
| **Workflow hiện tại (giả thuyết)** | 1. Bác sĩ mở hồ sơ bệnh án (1–3 phút). 2. Rà soát thông tin lâm sàng (8–15 phút). 3. Soạn tóm tắt (8–15 phút). 4. Kiểm tra và hoàn thiện (3–7 phút). Tổng thao tác: 20–40 phút/bản. |
| **Bottleneck cần xác minh** | Tổng hợp thông tin từ nhiều phần hồ sơ và viết lại thành tài liệu mạch lạc; giả thuyết chiếm 16–30 phút ở bước rà soát và soạn thảo. Cần đo thời gian bác sĩ thực tế dành cho tác vụ này. |
| **AI có thể hỗ trợ** | Tạo bản nháp từ dữ liệu được phép sử dụng, nêu thông tin thiếu hoặc mâu thuẫn để bác sĩ kiểm tra. |
| **Metric thử nghiệm** | Giả thuyết baseline thao tác: 20–40 phút/bản tóm tắt. Mục tiêu đề xuất: thời gian bác sĩ hoàn thiện bản nháp **≤5 phút/bệnh nhân**, đồng thời **100%** bản tóm tắt được bác sĩ duyệt trước khi phát hành. Cần đo baseline và xác lập tiêu chí chất lượng lâm sàng trước pilot. |
| **Quick Architecture** | LLM tạo bản nháp với bác sĩ trong vòng kiểm soát (Human-in-the-loop); không chẩn đoán, kê đơn hoặc tự phát hành tài liệu. |

## Kết luận lựa chọn

Tôi chọn **Quick Problem Card #2 — Vinhomes: Phân loại và chuyển tiếp phản ánh cư dân** làm ứng viên cho giai đoạn phân tích tiếp theo.

Lý do lựa chọn:
1. **Phạm vi tác vụ rõ:** đầu vào là nội dung phản ánh; đầu ra cần đánh giá là chủ đề, mức độ ưu tiên và bộ phận nhận ticket.
2. **Có thể đánh giá bằng dữ liệu:** ticket đã xử lý có thể được ẩn danh và gán nhãn để đo độ chính xác phân loại, độ chính xác định tuyến và thời gian xử lý.
3. **Có thể giới hạn quyền của AI:** AI chỉ đề xuất; nhân viên CSKH xác nhận trước khi chuyển. Trường hợp khẩn cấp, thiếu thông tin hoặc không chắc chắn quay về quy trình xử lý thủ công.
4. **Cần kiểm tra AI fit thay vì mặc định dùng LLM:** danh mục ổn định có thể phù hợp với rule-based; LLM chỉ có giá trị nếu xử lý tốt nội dung tự do và cải thiện kết quả so với baseline.

Trước khi chốt giải pháp, cần xác nhận với stakeholder danh mục ticket và bộ phận nhận xử lý, quy trình chuyển tuyến hiện tại, khối lượng ticket, baseline thời gian/tỷ lệ chuyển sai, tiêu chí khẩn cấp, và yêu cầu bảo vệ dữ liệu cư dân. Kết quả Phase 1–2 là lựa chọn ứng viên để nghiên cứu tiếp, chưa phải kết luận rằng giải pháp AI đã khả thi.
