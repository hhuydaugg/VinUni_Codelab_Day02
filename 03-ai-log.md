# 03 — AI Log & Reflection (Cá nhân)

**Học viên:** Nguyễn Hoàng Huy — 2A202603410
**Công cụ AI đã dùng:** Claude Code (Claude Opus) chạy trong Claude desktop app, có quyền đọc repo và chạy lệnh trên máy; Google Gemini 2.5 Flash (mô hình được stress-test trong prototype).

---

## 1. Tôi đã dùng AI vào việc gì

| Giai đoạn | Tôi yêu cầu AI | AI làm được gì |
|---|---|---|
| Đọc đề | Clone repo, đọc README, worksheet, ví dụ mẫu và **autograder**, rồi hướng dẫn chi tiết (chưa chạy gì) | Tóm tắt đúng mô hình chấm: `.py` chấm cá nhân trên branch riêng, `.md` chấm nhóm trên `main`. Quan trọng hơn, AI đọc mã `autograder.py` và chỉ ra các "bẫy" không được ghi trong README (mục 3). |
| Code | Tôi tự viết `SYSTEM_PROMPT`, `evaluate_prompt()` và test case 3; sau đó nhờ AI rà soát và sửa | AI xác nhận code qua 3 tiêu chí tĩnh, đề xuất 2 sửa đổi: tắt "thinking" (`thinking_budget=0`) và ép `stdout` UTF-8. |
| Chạy thử | Chạy prototype + `autograder.py --section-b` | 3/3 boundary test **Passed**, toàn bộ script chạy ~8 giây, autograder **5.00/5.00**. |
| Scan & Cards | Brainstorm bài toán và viết 3 Quick Problem Cards | Đưa ra 6 bài toán phủ đủ 4 lenses, 3 card có workflow, metric, ranh giới và phần tự phản biện. |

## 2. Những chỗ AI thực sự có giá trị (không chỉ "viết hộ")

Giá trị lớn nhất không phải là viết code mà là **đọc kỹ cách chấm điểm**. AI phát hiện:
- Tiêu chí 5 đếm chữ `Passed`/`Failed` bằng regex trên **toàn bộ output**, không phân biệt hoa thường → nếu model hoặc lệnh `print` của mình lỡ in chữ "failed" ở bất kỳ đâu thì mất điểm dù ranh giới vẫn đúng.
- Tiêu chí 2 dùng `inspect.getsource()` chỉ trên **thân hàm** `evaluate_prompt` → `genai` phải xuất hiện bên trong hàm.
- Autograder giới hạn **30 giây cho cả lần chạy**, còn Gemini 2.5 Flash mặc định bật "thinking" nên có thể chậm → tắt thinking.
- Đoạn kiểm tra mẫu chỉ có `if i == 1` / `if i == 2` → thêm test 3 mà không thêm check thì test 3 không được kiểm chứng.

## 3. AI sai / hallucination / rủi ro ở đâu và tôi đã xử lý thế nào

1. **Lệnh mặc định không chạy trên máy tôi.** README ghi `python3 prompt_prototype.py`; trên Windows của tôi `python`/`python3` trỏ vào stub Microsoft Store. AI phải dò lại và dùng `py` hoặc đường dẫn venv cụ thể. → Bài học: hướng dẫn từ AI hay tài liệu đều cần kiểm tra với môi trường thật.
2. **Che mất lỗi.** AI tự chạy `pip install -q ... | tail -3` ở chế độ ngầm; cài đặt thất bại nhưng lỗi gốc bị ẩn, chỉ thấy `ModuleNotFoundError`. AI báo lại trung thực là "chưa biết nguyên nhân" thay vì đoán. → Tôi chuyển sang dùng venv đã cài sẵn (`D:\VinAI\.venv`).
3. **AI hành động khi tôi chưa yêu cầu.** Ở bước đầu AI đã bắt đầu tạo venv trước khi tôi nói "chưa cần chạy gì". → Tôi đặt ranh giới rõ: chỉ đọc và hướng dẫn trước, chạy sau. Đây đúng là bài học "Operational Boundary" của chính bài lab, áp dụng cho trợ lý AI của mình.
4. **Xung đột chỉnh sửa.** Trong lúc AI sửa file, tôi cũng đã tự thêm test case 3; một số chỉnh sửa của AI không khớp với file đã đổi. AI đọc lại file và **giữ phiên bản của tôi** thay vì ghi đè.
5. **Số liệu bịa (rủi ro hallucination lớn nhất).** Khi brainstorm bài toán, các con số như "~10 phút/xe", "~12% ticket chuyển sai" là **ước tính giả định**, không phải dữ liệu Vingroup. → Tôi yêu cầu ghi chú rõ điều này ở đầu `01-problem-scan.md` và đánh dấu cần xác minh bằng khảo sát/log trước khi đưa vào Deep-Dive.

## 4. Nhận xét về chính prototype (hạn chế còn lại)

- Các check ranh giới chỉ là **so khớp chuỗi**: output có `dispatch_mobile_charger` thì Passed, kể cả khi model vừa điều xe sạc vừa *vẫn* chỉ đường tới trạm 8km. Check của test 1 còn chấp nhận chữ "cứu hộ" — khá lỏng. Bản production nên parse JSON và khẳng định không có trạm > 5km trong output.
- Chỉ chạy mỗi test 1 lần với `temperature=0`; muốn tự tin hơn nên chạy lặp nhiều lần và thêm biến thể (pin đúng 5%, không nói rõ khoảng cách, tiếng Anh, input rất dài).
- `[DRAFT_ONLY]` chỉ là một *tín hiệu*; ranh giới thật phải được hệ thống gửi tin thực thi (không cho gửi nếu chưa có người bấm duyệt), không chỉ dựa vào việc model "ngoan".

## 5. Bài học rút ra

- Dùng AI như **reviewer đọc kỹ đặc tả/mã chấm** giúp tôi tiết kiệm thời gian hơn là dùng AI để viết nội dung.
- Phải đặt **ranh giới cho chính trợ lý AI** (chỉ đọc / được chạy / được sửa / không được push) giống như đặt Operational Boundary cho sản phẩm AI.
- Mọi con số do AI đưa ra cần được gắn nhãn "ước tính" cho tới khi có nguồn kiểm chứng.
