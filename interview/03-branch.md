# Branch Interview Cases

## Case 01 - Feature Branch bị tụt phía sau main

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 5/5 |
| Mastery | 4/5 |
| Expected Level | Mid |
| Topics | Branch, Merge, Rebase, Remote |

### Scenario

`feature/login` đang chậm hơn `main` 20 Commit và sắp tạo Pull Request.

### Possible Solutions

#### Solution A - Merge

```bash
git fetch origin
git merge origin/main
```

#### Solution B - Rebase

```bash
git fetch origin
git rebase origin/main
```

### Recommended Answer

Nếu Feature Branch cá nhân và chưa chia sẻ, ưu tiên Rebase để lịch sử tuyến tính. Nếu Branch có nhiều người cùng làm, ưu tiên Merge để tránh Rewrite History.

### Why

Quyết định phụ thuộc ownership của Branch và quy ước nhóm, không có đáp án tuyệt đối.

### Interviewer Notes

Hỏi ứng viên Branch đã Push chưa, có người khác dựa trên Branch không và CI yêu cầu gì.

### Evaluation Rubric

- Junior: biết Merge.
- Mid: biết cả Merge và Rebase.
- Senior: hỏi về Shared Branch trước khi chọn.
- Staff: phân tích trade-off về review, audit và rollback.

---

## Case 02 - Xóa Branch sau khi Merge

| Metadata | Value |
|---|---|
| Difficulty | 1/5 |
| Frequency | 5/5 |
| Mastery | 2/5 |
| Expected Level | Junior |
| Topics | branch deletion, merged state |

### Scenario

Feature đã Merge vào `main`. Bạn muốn dọn Branch local và Remote.

### Possible Solutions

```bash
git branch -d feature/login
git push origin --delete feature/login
```

### Recommended Answer

Dùng `-d` trước để Git kiểm tra Branch đã Merge. Chỉ dùng `-D` khi hiểu rõ Commit chưa Merge có thể mất Reference.

### Why

Xóa Branch chỉ xóa Reference; Commit vẫn có thể còn reachable qua Branch khác hoặc Reflog.

### Interviewer Notes

Hỏi tiếp: xóa Branch có xóa Commit không?

### Evaluation Rubric

- Junior: biết xóa Branch.
- Mid: phân biệt `-d` và `-D`.
- Senior: giải thích Reachability.

---

## Case 03 - Làm việc song song trên hai Branch

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 3/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | worktree, branch, concurrent work |

### Scenario

Bạn cần chạy test dài trên Feature Branch nhưng đồng thời sửa Hotfix trên `main` mà không muốn Stash hoặc chuyển đổi thư mục liên tục.

### Possible Solutions

```bash
git worktree add ../repo-hotfix -b hotfix/payment main
```

Hoặc Clone Repository lần hai.

### Recommended Answer

Dùng Git Worktree nếu hai Working Tree cùng dùng một Object Database là phù hợp. Clone riêng nếu cần cô lập hoàn toàn config hoặc Remote.

### Why

Worktree tối ưu dung lượng và giảm thao tác chuyển Branch.

### Interviewer Notes

Hỏi tiếp: một Branch có thể được Checkout trong hai Worktree không?

### Evaluation Rubric

- Mid: đề xuất Clone hoặc Stash.
- Senior: biết Worktree.
- Staff: hiểu giới hạn Branch checkout và quản lý Worktree lifecycle.

---

## Case 04 - Branch được tạo từ sai base

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | rebase --onto, branch ancestry |

### Scenario

Bạn tạo `feature/b` từ `feature/a` thay vì từ `main`. `feature/a` có 10 Commit không liên quan, còn `feature/b` có 3 Commit riêng.

### Possible Solutions

```bash
git rebase --onto main feature/a feature/b
```

Hoặc Cherry-pick ba Commit sang Branch mới.

### Recommended Answer

Nếu xác định rõ điểm tách, dùng `rebase --onto`. Nếu lịch sử phức tạp hoặc cần kiểm soát từng Commit, tạo Branch mới từ `main` và Cherry-pick.

### Why

`--onto` phát lại phạm vi Commit của `feature/b` lên base mới mà không mang theo Commit của `feature/a`.

### Interviewer Notes

Yêu cầu ứng viên vẽ Commit Graph trước và sau.

### Evaluation Rubric

- Mid: tạo Branch mới và Cherry-pick.
- Senior: dùng `rebase --onto` đúng phạm vi.
- Staff: xác minh Merge Base và rủi ro Rewrite Branch đã chia sẻ.
