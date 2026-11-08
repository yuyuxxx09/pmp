# PMP Luyện Thi Song Ngữ

Ứng dụng HTML/CSS/JavaScript thuần; dữ liệu học và ngân hàng câu hỏi được lưu trong `localStorage` của trình duyệt.

## Thêm câu hỏi

Mở tab **Thêm câu hỏi**, chọn một trong tám dạng rồi điền các trường tương ứng. Form cấu hình động: dạng lựa chọn dùng bốn phương án song ngữ; Matching, Enhanced Matching, Hotspot và Pull-down List dùng JSON cấu hình có ví dụ sẵn trong form. Hotspot dùng tọa độ chuẩn hóa 0–1000; hình ảnh nên là đường dẫn tương đối trong dự án. Graphic-Based yêu cầu hình ảnh và mô tả ảnh.

Case / Scenario lưu nội dung tình huống cùng câu hỏi con; có thể chọn câu con một hoặc nhiều đáp án. Để liên kết nhiều câu với cùng tình huống, tạo từng câu với cùng một `Case ID` và nội dung tình huống giống hệt nhau. Các câu mới có `schema_version: 2` và `question_type`; chúng được thêm vào mảng `pmp_questions`. Câu hỏi legacy tiếp tục được đọc qua `type` hiện tại, hoặc mặc định `MCQ` khi thiếu `type`. Ứng dụng vẫn chạy các migration lưu trữ sẵn có, nhưng không bắt buộc dữ liệu cũ phải có `question_type`.

Ngân hàng câu hỏi có dạng:

```json
{
  "id": "Q013",
  "schema_version": 2,
  "question_type": "single_choice",
  "type": "MCQ",
  "question": "Question in English",
  "question_vi": "Câu hỏi bằng tiếng Việt",
  "options": { "A": "...", "B": "...", "C": "...", "D": "..." },
  "options_vi": { "A": "...", "B": "...", "C": "...", "D": "..." },
  "correctAnswer": "D",
  "explanation": {
    "correct_vi": "Giải thích đáp án đúng bằng tiếng Việt.",
    "correct_en": "Explanation of the correct answer in English.",
    "whyWrong_vi": { "A": "Lý do A sai.", "B": "Lý do B sai.", "C": "Lý do C sai." },
    "pmp_mindset_vi": "Tư duy cốt lõi cần nhớ."
  }
}
```

Các giá trị `question_type` được hỗ trợ: `single_choice`, `multiple_response`, `case_scenario`, `graphic_based`, `matching`, `enhanced_matching`, `hotspot`, `pull_down_list`. Ứng dụng ánh xạ các dạng mới sang renderer/chấm điểm hiện có khi có thể; schema legacy `type`, `matchingPairs`, `matchingOptions`, `dropdowns` vẫn được đọc. Case dùng `case_id`, `scenario`, `scenario_vi`; Graphic-Based dùng `image`, `image_alt`; dạng tương tác khác dùng field tương ứng `matching`, `enhanced_matching`, `hotspot`, hoặc `dropdowns`/`correctAnswer`.

Các câu hỏi cũ không có `pmp_mindset_vi` vẫn được tải bình thường; phần mới sẽ chỉ hiển thị khi có nội dung. Có thể sao lưu toàn bộ dữ liệu bằng **Settings → Export JSON**.

## Dashboard và Review

Dashboard giữ các danh sách dài trong vùng cuộn; Review gộp các phiên Practice đã trả lời với các bài Exam đã nộp, hiển thị nhãn nguồn và phân trang 10 câu/lượt. Trường `mode` được bổ sung vào dữ liệu trả lời mới (`practice`/`exam`); bản ghi cũ vẫn được đọc bằng `source` và cấu trúc hiện tại.
