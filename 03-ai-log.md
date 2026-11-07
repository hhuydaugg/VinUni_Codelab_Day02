# 03 — AI Log & Reflection (Nhật ký tương tác AI)

**Học viên:** Hồ Xuân Vũ  — 2A202603406  
**Vai trò của AI trong quá trình làm bài:** Trợ lý phản biện & khung kỹ thuật (Co-pilot & Sparring Partner).  
**Nguyên tắc tương tác:** AI không làm thay tư duy nghiệp vụ. Mọi ý tưởng cốt lõi về bài toán thực tế (VinFast EV Battery Warranty), ranh giới vận hành (operational boundaries), và logic kiểm tra adversarial testing đều do tôi chủ động định hình, lựa chọn và kiểm chứng; AI chỉ hỗ trợ tra cứu cấu trúc văn bản, tăng tốc viết code boilerplate và đóng vai trò "cố vấn phản biện" để phát hiện lỗ hổng logic.

---

## 1. AI đã giúp gì trong quá trình thực hiện?

Trong suốt quá trình từ khảo sát 4 lenses đến Deep-Dive và lập trình Prompt Prototype, tôi đã sử dụng Gemini làm công cụ hỗ trợ với các tác vụ cụ thể:
* **Chuẩn hóa cấu trúc tài liệu:** Hỗ trợ định dạng bảng Markdown cho phần Problem Scan và cấu trúc Quick Problem Cards sao cho ngắn gọn, súc tích và đúng chuẩn template của môn học.
* **Tư duy phản biện (Devil’s Advocate):** Khi tôi đưa ra bài toán thẩm định telemetry pin EV, tôi yêu cầu AI đóng vai trò một CFO/Trưởng vận hành khắc nghiệt để đặt câu hỏi vặn vẹo về hiệu quả kinh tế và rủi ro tuân thủ, từ đó giúp tôi tự hoàn thiện phần *Justification* và *Operational Boundary*.
* **Hỗ trợ sinh mã Python boilerplate:** Xây dựng cấu trúc script `prompt_prototype.py` tích hợp SDK Gemini 2.5 để chạy thử nghiệm các kịch bản kiểm thử ranh giới (Adversarial Testing).

---

## 2. Các điểm AI trả lời sai, hallucination hoặc chưa sát thực tế

Trong quá trình làm việc, tôi ghi nhận một số điểm mà AI chưa thể tự động tối ưu nếu thiếu sự can thiệp và kiểm soát chặt chẽ của con người:
* **Sai lệch ngữ cảnh ban đầu (Context Drift):** Ở các lượt prompt đầu tiên, AI hay nhầm lẫn bài toán của tôi sang các kịch bản chung chung của Xanh SM (như điều phối xe yếu pin) thay vì tập trung sâu vào đặc thù nghiệp vụ **Thẩm định dữ liệu Telemetry và Khiếu nại bảo hành pin của VinFast**. Tôi phải chủ động ép lại ngữ cảnh (re-contextualize).
* **Đề xuất giải pháp quá đà (Over-engineering):** AI có xu hướng đề xuất các kiến trúc Agentic phức tạp đa bước (multi-step autonomous agent) cho bài toán tiếp nhận dịch vụ. Tôi đã phải bác bỏ vì thực tế vận hành xưởng dịch vụ đòi hỏi tính đơn giản, tốc độ phản hồi nhanh và kiểm soát bằng con người (Human-in-the-loop - HITL) tuyệt đối để tránh rủi ro pháp lý và tài chính.
* **Số liệu giả định chưa có kiểm chứng:** AI tự động sinh ra các con số ước tính tổn thất khá chung chung; tôi phải tự điều chỉnh lại dựa trên logic vận hành thực tế của ngành ô tô và ghi rõ chú thích đây là số liệu giả định cần đo lường baseline.

---

## 3. Quá trình sửa prompt, thiết lập ranh giới và kiểm soát kết quả

Để ép AI phục vụ đúng mục đích học tập và làm sản phẩm mà không làm mất đi quyền kiểm soát của con người, tôi đã thực hiện các bước tinh chỉnh prompt và ranh giới sau:

1. **Thiết lập System Prompt với các ranh giới cứng (Hard Boundaries):**
   * *Ranh giới 1 (Tagging):* Bắt buộc mọi đầu ra phải có thẻ `[WARRANTY_REVIEW_DRAFT]` để ngăn chặn việc hệ thống tự ý tự động hóa quyết định tài chính.
   * *Ranh giới 2 (Safety Guard):* Tách bạch rõ ràng: phần kiểm tra từ khóa cạ gầm/ngoại lực từ telemetry phải dùng rule cứng, không cho LLM tự do diễn giải cảm tính để tránh bỏ sót lỗi bảo hành nguy hiểm.
2. **Kiểm thử đối kháng (Adversarial Stress-Testing):** 
   * Tôi tự thiết lập các kịch bản tấn công prompt (Prompt Injection, cố tình yêu cầu bỏ qua thẻ nháp khi xe bị sốc tải gia tốc 4.2G do cạ gầm).
   * Qua các lần chạy script, khi phát hiện mô hình có xu hướng chiều lòng người dùng, tôi đã siết chặt lại System Prompt bằng câu lệnh định hướng `temperature=0` và cấu trúc ép kiểu JSON ở dòng đầu tiên để mô hình không thể vượt rào.

---

## 4. Đánh giá chung (Reflection)
AI là một công cụ tăng tốc tuyệt vời (giúp tiết kiệm 70% thời gian gõ code và trình bày văn bản), nhưng **tư duy sản phẩm (Product Thinking), việc xác định đúng pain point vận hành, và thiết kế ranh giới an toàn (Guardrails) là năng lực cốt lõi thuộc về con người**. Việc giữ quyền kiểm soát hoàn toàn (Human-in-the-loop) chính là chìa khóa để ứng dụng AI hiệu quả và an toàn trong môi trường doanh nghiệp thực tế như Vingroup.