# pmp
# PMP Luyện Thi Song Ngữ

Ứng dụng HTML/CSS/JavaScript thuần; dữ liệu học và ngân hàng câu hỏi được lưu trong `localStorage` của trình duyệt.

## Thêm câu hỏi

Mở tab **Thêm câu hỏi** trong ứng dụng, điền nội dung tiếng Anh/tiếng Việt, bốn lựa chọn, đáp án đúng và phần giải thích. Trường “Tại sao sai” chỉ yêu cầu điền cho các lựa chọn sai. Câu mới được thêm vào mảng `pmp_questions`; bấm **Luyện câu hỏi** để bắt đầu một lượt Practice mới.

Ngân hàng câu hỏi có dạng:

```json
{
  "id": "Q013",
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

Các câu hỏi cũ không có `pmp_mindset_vi` vẫn được tải bình thường; phần mới sẽ chỉ hiển thị khi có nội dung. Có thể sao lưu toàn bộ dữ liệu bằng **Settings → Export JSON**.
