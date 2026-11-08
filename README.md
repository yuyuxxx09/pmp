# PMP Luyện Thi Song Ngữ

Ứng dụng HTML/CSS/JavaScript thuần; dữ liệu học và ngân hàng câu hỏi được lưu trong `localStorage` của trình duyệt.

## Thêm câu hỏi

Mở tab **Thêm câu hỏi**, chọn một trong tám dạng rồi điền các trường tương ứng. Với dạng lựa chọn, form mặc định có bốn phương án song ngữ; dùng **Thêm lựa chọn** hoặc **Xóa** để tạo từ 2 đến 26 phương án A–Z. Multiple Response cho phép chọn nhiều đáp án đúng. Matching, Enhanced Matching, Hotspot và Pull-down List dùng JSON cấu hình có ví dụ sẵn trong form. Hotspot dùng tọa độ chuẩn hóa 0–1000; hình ảnh nên là đường dẫn tương đối trong dự án. Graphic-Based yêu cầu hình ảnh và mô tả ảnh.

Case / Scenario lưu nội dung tình huống cùng câu hỏi con; có thể chọn câu con một hoặc nhiều đáp án. Để liên kết nhiều câu với cùng tình huống, tạo từng câu với cùng một `Case ID` và nội dung tình huống giống hệt nhau. Các câu mới có `schema_version: 2` và `question_type`; chúng được thêm vào mảng `pmp_questions`. Câu hỏi legacy tiếp tục được đọc qua `type` hiện tại, hoặc mặc định `MCQ` khi thiếu `type`. Ứng dụng vẫn chạy các migration lưu trữ sẵn có, nhưng không bắt buộc dữ liệu cũ phải có `question_type`.

Mỗi câu hỏi dùng chung các trường `id`, `schema_version`, `question_type`, `type`, `domain`, `task`, `question`, `question_vi` và `explanation`. `question_type` quyết định các trường riêng theo loại; `type` là tên tương thích với renderer cũ. Các mẫu dưới đây là object câu hỏi riêng lẻ; có thể ghép nhiều object vào một mảng để import.

```json
{
  "id": "Q013",
  "schema_version": 2,
  "question_type": "single_choice",
  "type": "MCQ",
  "domain": "Process",
  "task": "Task 1: Manage conflict",
  "question": "Question in English",
  "question_vi": "Câu hỏi bằng tiếng Việt",
  "options": { "A": "Option A", "B": "Option B", "C": "Option C", "D": "Option D" },
  "options_vi": { "A": "Lựa chọn A", "B": "Lựa chọn B", "C": "Lựa chọn C", "D": "Lựa chọn D" },
  "correctAnswer": "D",
  "explanation": {
    "correct_vi": "Giải thích đáp án đúng bằng tiếng Việt.",
    "correct_en": "Explanation of the correct answer in English.",
    "whyWrong_vi": {
      "A": "Lý do A sai.",
      "B": "Lý do B sai.",
      "C": "Lý do C sai."
    },
    "pmp_mindset_vi": "Tư duy cốt lõi cần nhớ."
  }
}
```

### Các trường riêng theo dạng

Các ví dụ bên dưới chỉ liệt kê phần khác với trường chung ở trên. Khi tạo câu mới, thay `question_type`, `type` và thêm các trường riêng tương ứng vào object chung. Mọi dạng lựa chọn dùng `options` và `options_vi` với cùng các key từ A đến Z (ít nhất 2 lựa chọn). `correctAnswer` là một key cho câu một đáp án, hoặc chuỗi các key phân cách bằng dấu phẩy cho câu nhiều đáp án.

**1. Single Choice** — một đáp án đúng:

```json
{
  "question_type": "single_choice",
  "type": "MCQ",
  "options": { "A": "...", "B": "...", "C": "...", "D": "...", "E": "..." },
  "options_vi": { "A": "...", "B": "...", "C": "...", "D": "...", "E": "..." },
  "correctAnswer": "E",
  "explanation": {
    "correct_vi": "...", "correct_en": "...",
    "whyWrong_vi": { "A": "...", "B": "...", "C": "...", "D": "..." },
    "pmp_mindset_vi": "..."
  }
}
```

**2. Multiple Response** — nhiều đáp án đúng; ví dụ chọn 2 đáp án trong 5:

```json
{
  "question_type": "multiple_response",
  "type": "MultipleResponse",
  "options": { "A": "...", "B": "...", "C": "...", "D": "...", "E": "..." },
  "options_vi": { "A": "...", "B": "...", "C": "...", "D": "...", "E": "..." },
  "correctAnswer": "B,E",
  "explanation": {
    "correct_vi": "...", "correct_en": "...",
    "whyWrong_vi": { "A": "...", "C": "...", "D": "..." },
    "pmp_mindset_vi": "..."
  }
}
```

**3. Case / Scenario** — mỗi câu hỏi con lưu tình huống cùng `case_id`. Dùng cùng `case_id` và cùng nội dung tình huống cho các câu thuộc một case; `type` có thể là `MCQ` hoặc `MultipleResponse`:

```json
{
  "question_type": "case_scenario",
  "type": "MultipleResponse",
  "case_id": "CASE-01",
  "scenario": "Shared case text in English.",
  "scenario_vi": "Nội dung tình huống dùng chung.",
  "options": { "A": "...", "B": "...", "C": "...", "D": "...", "E": "..." },
  "options_vi": { "A": "...", "B": "...", "C": "...", "D": "...", "E": "..." },
  "correctAnswer": "A,C",
  "explanation": {
    "correct_vi": "...", "correct_en": "...",
    "whyWrong_vi": { "B": "...", "D": "...", "E": "..." },
    "pmp_mindset_vi": "..."
  }
}
```

**4. Graphic-Based** — dạng lựa chọn kèm hình/biểu đồ:

```json
{
  "question_type": "graphic_based",
  "type": "MCQ",
  "image": "assets/chart.png",
  "image_alt": "Mô tả nội dung biểu đồ",
  "options": { "A": "...", "B": "...", "C": "...", "D": "..." },
  "options_vi": { "A": "...", "B": "...", "C": "...", "D": "..." },
  "correctAnswer": "C",
  "explanation": {
    "correct_vi": "...", "correct_en": "...",
    "whyWrong_vi": { "A": "...", "B": "...", "D": "..." },
    "pmp_mindset_vi": "..."
  }
}
```

**5. Matching** — ghép mỗi prompt với một lựa chọn. `correctAnswer` dạng chuỗi JSON phải tương ứng với `matching.answer_map`:

```json
{
  "question_type": "matching",
  "type": "Matching",
  "matching": {
    "prompts": [
      { "id": "P1", "text": { "en": "Resolve conflict", "vi": "Giải quyết xung đột" } },
      { "id": "P2", "text": { "en": "Build agreement", "vi": "Xây dựng đồng thuận" } }
    ],
    "choices": [
      { "id": "C1", "text": { "en": "Collaborate", "vi": "Hợp tác" } },
      { "id": "C2", "text": { "en": "Facilitate discussion", "vi": "Điều phối thảo luận" } }
    ],
    "answer_map": { "P1": "C1", "P2": "C2" },
    "reuse_choices": false
  },
  "correctAnswer": "{\"P1\":\"C1\",\"P2\":\"C2\"}",
  "explanation": {
    "correct_vi": "...", "correct_en": "...",
    "whyWrong_vi": {},
    "pmp_mindset_vi": "..."
  }
}
```

**6. Enhanced Matching** — ghép item vào target; mỗi target phải có item riêng:

```json
{
  "question_type": "enhanced_matching",
  "type": "EnhancedMatching",
  "enhanced_matching": {
    "targets": [
      { "id": "T1", "text": { "en": "People", "vi": "Con người" } },
      { "id": "T2", "text": { "en": "Process", "vi": "Quy trình" } }
    ],
    "items": [
      { "id": "I1", "text": { "en": "Coach the team", "vi": "Huấn luyện nhóm" } },
      { "id": "I2", "text": { "en": "Analyze the process", "vi": "Phân tích quy trình" } }
    ],
    "answer_map": { "T1": "I1", "T2": "I2" }
  },
  "explanation": {
    "correct_vi": "...", "correct_en": "...",
    "whyWrong_vi": {},
    "pmp_mindset_vi": "..."
  }
}
```

**7. Hotspot** — chọn vùng trên hình. Tọa độ dùng thang 0–1000; `correct_region_ids` có một hoặc nhiều ID tùy `selection_mode`:

```json
{
  "question_type": "hotspot",
  "type": "Hotspot",
  "hotspot": {
    "image": "assets/chart.png",
    "image_alt": "Control chart",
    "selection_mode": "single",
    "regions": [
      { "id": "R1", "shape": "rect", "coords": [100, 100, 200, 150], "label": "Point A" },
      { "id": "R2", "shape": "polygon", "coords": [[400, 200], [600, 200], [550, 400]], "label": "Point B" }
    ],
    "correct_region_ids": ["R2"]
  },
  "correctAnswer": "R2",
  "explanation": {
    "correct_vi": "...", "correct_en": "...",
    "whyWrong_vi": {},
    "pmp_mindset_vi": "..."
  }
}
```

**8. Pull-down List** — câu hỏi có placeholder `[1]`, `[2]`... theo thứ tự. `correctAnswer` lưu key đúng của từng danh sách, phân cách bằng dấu phẩy:

```json
{
  "question_type": "pull_down_list",
  "type": "Dropdown",
  "question": "Choose [1] first, then [2].",
  "question_vi": "Chọn [1] trước, sau đó chọn [2].",
  "dropdowns": [
    { "options": [
      { "key": "A", "text": "Avoid", "text_vi": "Tránh" },
      { "key": "B", "text": "Mitigate", "text_vi": "Giảm thiểu" }
    ] },
    { "options": [
      { "key": "A", "text": "Accept", "text_vi": "Chấp nhận" },
      { "key": "B", "text": "Transfer", "text_vi": "Chuyển giao" }
    ] }
  ],
  "correctAnswer": "B,A",
  "explanation": {
    "correct_vi": "...", "correct_en": "...",
    "whyWrong_vi": {},
    "pmp_mindset_vi": "..."
  }
}
```

**Lưu ý khi import:** Mỗi mẫu cần được ghép với các trường chung ở phần đầu (đặc biệt là `id`, `schema_version: 2`, `domain`, `task`, `question`, `question_vi`, và đầy đủ `explanation`). Có thể import một mảng JSON các câu hỏi hoặc object `{ "schema_version": 2, "questions": [...] }`. Với Matching, `correctAnswer` phải là chuỗi JSON như ví dụ; với Pull-down List, đó là các key phân cách bằng dấu phẩy. Câu legacy vẫn có thể dùng `type`, `matchingPairs`, `matchingOptions` và `dropdowns` mà không cần `question_type`.

Các câu hỏi cũ không có `pmp_mindset_vi` vẫn được tải bình thường; phần mới sẽ chỉ hiển thị khi có nội dung. Có thể sao lưu toàn bộ dữ liệu bằng **Settings → Export JSON**.

## Dashboard và Review

Dashboard giữ các danh sách dài trong vùng cuộn; Review gộp các phiên Practice đã trả lời với các bài Exam đã nộp, hiển thị nhãn nguồn và phân trang 10 câu/lượt. Trường `mode` được bổ sung vào dữ liệu trả lời mới (`practice`/`exam`); bản ghi cũ vẫn được đọc bằng `source` và cấu trúc hiện tại.
