# Conflict Interview Cases

## Case 01 - Conflict khi Merge

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 5/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | merge conflict, index stages |

### Scenario

Merge dừng vì Conflict trong hai tệp.

### Recommended Answer

```bash
git status
# sửa conflict
git add <resolved-files>
git diff --staged
git commit
```

Hoặc `git merge --abort` nếu cần quay lại trạng thái trước Merge.

### Why

Ứng viên cần review kết quả cuối, không chỉ xóa conflict markers.

### Interviewer Notes

Hỏi tiếp: `ours` và `theirs` nghĩa gì trong Merge?

### Evaluation Rubric

- Junior: sửa markers.
- Mid: dùng status, add và commit.
- Senior: kiểm tra semantic conflict và test sau resolve.

---

## Case 02 - Conflict không có marker trong code

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 3/5 |
| Mastery | 5/5 |
| Expected Level | Senior |
| Topics | semantic conflict, testing, review |

### Scenario

Merge hoàn tất không có textual conflict, nhưng hai Branch thay đổi hành vi theo cách không tương thích.

### Recommended Answer

Nhận diện đây là Semantic Conflict. Review Diff tổng, chạy test tích hợp, kiểm tra contract và trao đổi với tác giả thay đổi. Git không thể tự phát hiện mọi xung đột nghiệp vụ.

### Interviewer Notes

Đây là câu phân biệt người biết command với người hiểu quy trình tích hợp.

### Evaluation Rubric

- Mid: tin Merge thành công là đủ.
- Senior: chủ động tìm Semantic Conflict.
- Staff: đề xuất test hoặc contract ngăn lặp lại.

---

## Case 03 - Conflict khi Rebase

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | rebase conflict, ours/theirs semantics |

### Scenario

Trong Rebase, ứng viên dùng `git checkout --ours` nhưng nhận kết quả ngược với kỳ vọng.

### Recommended Answer

Giải thích trong Rebase, cách Git nhìn `ours` và `theirs` có thể gây nhầm vì Commit đang được replay lên upstream. Khuyến nghị xem nội dung cụ thể bằng `git checkout --conflict=merge`, `git show` và giải quyết theo ý nghĩa, không chọn nhãn máy móc.

### Interviewer Notes

Yêu cầu ứng viên giải thích Commit nào đang được áp dụng.

### Evaluation Rubric

- Mid: biết continue/abort.
- Senior: hiểu ngữ cảnh `ours/theirs` trong Rebase.
- Staff: tránh command shortcut khi chưa xác định semantic intent.

---

## Case 04 - Cùng Conflict lặp lại nhiều lần

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 2/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | rerere, repeated resolution |

### Scenario

Bạn thường xuyên resolve cùng một Conflict trên long-lived Branch.

### Possible Solutions

```bash
git config rerere.enabled true
```

Hoặc thay đổi chiến lược Branch để giảm Divergence.

### Recommended Answer

`rerere` có thể lưu và tái áp dụng Resolution, nhưng cần review kết quả. Đồng thời xem xét nguyên nhân quy trình khiến Conflict lặp lại.

### Interviewer Notes

Không đánh giá cao câu trả lời chỉ bật rerere mà không nói về review.

### Evaluation Rubric

- Senior: biết rerere.
- Staff: kết hợp công cụ với cải tiến branching strategy.
