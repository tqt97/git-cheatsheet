# Git Interview Case Studies

> Bộ câu hỏi tình huống dùng để đánh giá khả năng sử dụng, phân tích và xử lý sự cố Git trong môi trường làm việc thực tế.

## Mục tiêu

Phần này không tập trung vào câu hỏi học thuộc như "Merge khác Rebase thế nào?". Mỗi bài tập đặt ứng viên vào một tình huống thực tế, yêu cầu:

1. Phân tích trạng thái Repository.
2. Xác định rủi ro.
3. Đưa ra nhiều phương án khi có thể.
4. So sánh trade-off.
5. Chọn phương án phù hợp với ngữ cảnh.
6. Giải thích cơ chế Git phía sau quyết định.

## Hệ thống đánh giá

| Thuộc tính | Ý nghĩa |
|---|---|
| Difficulty | Độ khó kỹ thuật của tình huống, từ 1 đến 5 |
| Frequency | Mức độ thường gặp trong công việc, từ 1 đến 5 |
| Mastery | Khả năng phân biệt ứng viên chỉ biết dùng Git với ứng viên thực sự hiểu Git, từ 1 đến 5 |
| Expected Level | Cấp độ tối thiểu thường có thể xử lý tốt tình huống |
| Topics | Các chủ đề Git được kiểm tra |

## Thang điểm Mastery

| Điểm | Ý nghĩa |
|---|---|
| 1/5 | Kiểm tra kiến thức thao tác cơ bản |
| 2/5 | Phân biệt Junior và Mid |
| 3/5 | Phân biệt Mid và Senior |
| 4/5 | Phân biệt Senior và Staff |
| 5/5 | Yêu cầu hiểu sâu Git Internals, lịch sử và rủi ro cộng tác |

## Cấu trúc mỗi case

```text
Scenario
↓
Questions for Candidate
↓
Expected Analysis
↓
Possible Solutions
↓
Recommended Answer
↓
Why
↓
Interviewer Notes
↓
Evaluation Rubric
```

## Danh mục

1. [Working Tree](01-working-tree.md)
2. [Commit](02-commit.md)
3. [Branch](03-branch.md)
4. [Merge](04-merge.md)
5. [Rebase](05-rebase.md)
6. [Remote](06-remote.md)
7. [Conflict](07-conflict.md)
8. [Reset](08-reset.md)
9. [Recovery](09-recovery.md)
10. [Release](10-release.md)
11. [Team Collaboration](11-team-collaboration.md)
12. [Git Internals](12-git-internals.md)

## Cách sử dụng trong interview

- Không yêu cầu ứng viên đọc thuộc command.
- Yêu cầu ứng viên mô tả trạng thái Working Tree, Index, HEAD và Remote trước khi thao tác.
- Chấp nhận nhiều đáp án nếu ứng viên nêu rõ điều kiện áp dụng và rủi ro.
- Với câu hỏi có Rewrite History, luôn hỏi thêm Branch đã được Push hoặc chia sẻ chưa.
- Đánh giá cao ứng viên biết dừng lại để thu thập thông tin trước khi chạy lệnh phá hủy.
