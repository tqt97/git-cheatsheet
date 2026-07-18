# Reset Interview Cases

## Case 01 - Phân biệt soft, mixed và hard

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | reset modes, HEAD, Index, Working Tree |

### Scenario

Ứng viên cần bỏ Commit cuối nhưng có ba yêu cầu khác nhau: giữ thay đổi đã Stage, giữ thay đổi chưa Stage, hoặc xóa toàn bộ.

### Recommended Answer

- `git reset --soft HEAD~1`: di chuyển Branch, giữ Index và Working Tree.
- `git reset HEAD~1`: di chuyển Branch, cập nhật Index, giữ Working Tree.
- `git reset --hard HEAD~1`: cập nhật cả Branch, Index và Working Tree.

### Interviewer Notes

Yêu cầu vẽ trạng thái trước và sau.

### Evaluation Rubric

- Mid: nhớ command.
- Senior: giải thích tác động từng vùng.
- Staff: cảnh báo untracked files không bị xóa bởi reset hard.

---

## Case 02 - Reset một tệp khỏi Index

| Metadata | Value |
|---|---|
| Difficulty | 1/5 |
| Frequency | 5/5 |
| Mastery | 2/5 |
| Expected Level | Junior |
| Topics | unstage, restore --staged |

### Possible Solutions

```bash
git restore --staged file
```

hoặc cú pháp cũ:

```bash
git reset HEAD -- file
```

### Recommended Answer

Ưu tiên `git restore --staged` vì mục đích rõ ràng hơn.

### Evaluation Rubric

- Junior: biết bỏ Stage.
- Mid: giải thích Working Tree không đổi.

---

## Case 03 - Reset nhầm làm mất Commit local

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 3/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | reset, reflog, recovery |

### Scenario

Bạn chạy `git reset --hard HEAD~3` và nhận ra hai Commit cần được giữ.

### Recommended Answer

Không tạo thêm thay đổi. Dùng `git reflog` để tìm Commit trước Reset, tạo Branch bảo vệ hoặc Reset lại:

```bash
git reflog
git branch recovery/<name> <old-commit>
```

### Why

Reset xóa Reference hiện tại nhưng Commit thường vẫn còn trong Object Database và Reflog.

### Interviewer Notes

Ưu tiên ứng viên tạo Branch recovery trước khi tiếp tục.

### Evaluation Rubric

- Mid: biết Reflog.
- Senior: bảo vệ Commit bằng Branch.
- Staff: hiểu thời gian tồn tại phụ thuộc Reflog expiry và GC.

---

## Case 04 - Reset trên Branch đã Push

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 3/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | reset, revert, shared history |

### Scenario

Một Commit lỗi đã nằm trên `main` và nhiều người đã Pull.

### Possible Solutions

- Reset và Force Push.
- Revert Commit.

### Recommended Answer

Dùng Revert để tạo lịch sử đảo ngược mới. Không Reset Shared Branch trừ tình huống đặc biệt có phối hợp nghiêm ngặt.

### Interviewer Notes

Hỏi ứng viên tại sao "lịch sử sạch" không quan trọng bằng tính ổn định cộng tác.

### Evaluation Rubric

- Mid: biết Revert.
- Senior: hiểu public history.
- Staff: cân nhắc incident response và audit trail.
