# Rebase Interview Cases

## Case 01 - Rebase Feature Branch cá nhân

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 5/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | rebase, fetch, force-with-lease |

### Scenario

Feature Branch cá nhân đã Push và Pull Request chưa có người khác đóng góp. `main` vừa cập nhật.

### Possible Solutions

```bash
git fetch origin
git rebase origin/main
git push --force-with-lease
```

Hoặc Merge `origin/main` vào Feature.

### Recommended Answer

Rebase phù hợp nếu Branch thực sự thuộc sở hữu cá nhân và team chấp nhận Rewrite. Nếu không chắc, Merge an toàn hơn.

### Interviewer Notes

Hỏi ứng viên xác minh Branch ownership thế nào.

### Evaluation Rubric

- Mid: thực hiện Rebase.
- Senior: dùng `--force-with-lease`.
- Staff: cân nhắc CI, review comments và commit identity sau Rewrite.

---

## Case 02 - Interactive Rebase để chuẩn bị Pull Request

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | interactive rebase, squash, fixup, reword |

### Scenario

Feature Branch có 12 Commit gồm WIP, fix typo và sửa review. Bạn muốn chuẩn bị lịch sử dễ đọc trước khi Merge.

### Possible Solutions

```bash
git rebase -i origin/main
```

Dùng `reword`, `squash`, `fixup` hoặc sắp xếp lại Commit.

### Recommended Answer

Chỉ Rewrite khi Branch không dùng chung. Nhóm Commit theo thay đổi logic, không nhất thiết squash thành một Commit duy nhất.

### Why

Lịch sử tốt phải hỗ trợ review, bisect và revert, không chỉ "ít Commit".

### Interviewer Notes

Hỏi tiếp: khi nào không nên squash mọi thứ?

### Evaluation Rubric

- Mid: biết Interactive Rebase.
- Senior: thiết kế atomic Commit.
- Staff: giữ ranh giới phục vụ audit và rollback.

---

## Case 03 - Rebase gặp Conflict ở nhiều Commit

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 3/5 |
| Mastery | 5/5 |
| Expected Level | Senior |
| Topics | rebase conflict, continue, skip, abort |

### Scenario

Rebase 15 Commit lên `main`; Conflict xuất hiện lặp lại vì cùng vùng code thay đổi qua nhiều Commit.

### Possible Solutions

- Resolve từng Commit rồi `git rebase --continue`.
- Dùng `git rebase --abort`, Merge thay vì Rebase.
- Squash hoặc sắp xếp Commit trước khi Rebase.
- Bật `rerere` để tái sử dụng conflict resolution trong tình huống phù hợp.

### Recommended Answer

Đầu tiên đánh giá giá trị của lịch sử tuyến tính so với chi phí và rủi ro resolve nhiều lần. Nếu Branch cá nhân và lịch sử cần sạch, tiếp tục có kiểm soát; nếu Branch chia sẻ hoặc conflict phức tạp, Abort và Merge có thể phù hợp hơn.

### Interviewer Notes

Hỏi ứng viên `--skip` có thể làm mất gì.

### Evaluation Rubric

- Mid: biết continue và abort.
- Senior: không dùng skip mù quáng.
- Staff: biết `rerere` và đánh giá lại chiến lược thay vì cố hoàn tất bằng mọi giá.

---

## Case 04 - Rebase nhầm Shared Branch

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 2/5 |
| Mastery | 5/5 |
| Expected Level | Staff |
| Topics | shared history, recovery, communication |

### Scenario

Bạn đã Rebase và Force Push Shared Branch. Hai đồng đội có Commit dựa trên lịch sử cũ.

### Possible Solutions

- Khôi phục Remote Branch về lịch sử cũ từ Reflog, rồi phối hợp Rebase lại có kiểm soát.
- Giữ lịch sử mới và hướng dẫn đồng đội Rebase hoặc Cherry-pick Commit của họ.

### Recommended Answer

Dừng thêm Push, thông báo nhóm, xác định lịch sử chuẩn và bảo toàn mọi Commit trước. Chọn khôi phục lịch sử cũ nếu nhiều người bị ảnh hưởng; giữ lịch sử mới nếu ảnh hưởng nhỏ và có thể phối hợp rõ ràng.

### Why

Đây là sự cố cộng tác, không chỉ là bài toán command.

### Interviewer Notes

Đánh giá khả năng ưu tiên bảo toàn dữ liệu và giao tiếp.

### Evaluation Rubric

- Senior: biết dùng Reflog.
- Staff: lập kế hoạch phục hồi, ownership và communication trước khi thao tác.
