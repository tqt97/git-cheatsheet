# Recovery Interview Cases

## Case 01 - Xóa nhầm Branch chưa Merge

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 3/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | reflog, branch recovery |

### Scenario

Bạn dùng `git branch -D` xóa Feature Branch chưa Merge.

### Recommended Answer

Tìm Commit cuối trong Reflog hoặc Log còn tham chiếu, sau đó tạo lại Branch:

```bash
git reflog
git branch feature/recovered <commit>
```

### Interviewer Notes

Hỏi nếu Branch chỉ tồn tại trên máy đồng đội thì sao.

### Evaluation Rubric

- Mid: biết Reflog.
- Senior: tạo Reference bảo vệ.
- Staff: khảo sát Remote, CI refs và máy đồng đội trước khi kết luận mất dữ liệu.

---

## Case 02 - Commit bị mất sau Interactive Rebase

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 2/5 |
| Mastery | 5/5 |
| Expected Level | Senior |
| Topics | reflog, rebase recovery, object reachability |

### Scenario

Sau Interactive Rebase, một Commit quan trọng biến mất vì bị Drop nhầm.

### Recommended Answer

Dùng Reflog của HEAD hoặc Branch để xác định lịch sử trước Rebase, tạo Branch recovery, rồi Cherry-pick Commit cần thiết.

### Why

Rebase tạo chuỗi Commit mới; Commit cũ thường còn reachable qua Reflog trong một thời gian.

### Interviewer Notes

Hỏi ứng viên tại sao không Reset ngay khi chưa chắc Commit nào đúng.

### Evaluation Rubric

- Senior: dùng Reflog và Cherry-pick.
- Staff: bảo tồn cả hai lịch sử trước khi phục hồi.

---

## Case 03 - File bị xóa từ nhiều Commit trước

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 4/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | log -- path, restore source |

### Possible Solutions

```bash
git log -- path/to/file
git restore --source=<commit> -- path/to/file
```

Hoặc:

```bash
git checkout <commit> -- path/to/file
```

### Recommended Answer

Tìm Commit chứa phiên bản cần thiết, Restore tệp vào Working Tree, review rồi Commit lại.

### Evaluation Rubric

- Junior: tìm trong lịch sử thủ công.
- Mid: dùng path-limited log và restore source.
- Senior: xác minh lịch sử rename khi cần.

---

## Case 04 - Repository báo object corrupt

| Metadata | Value |
|---|---|
| Difficulty | 5/5 |
| Frequency | 1/5 |
| Mastery | 5/5 |
| Expected Level | Staff |
| Topics | fsck, object database, backup, recovery |

### Scenario

`git fsck` báo missing hoặc corrupt object trong Repository quan trọng.

### Recommended Answer

Dừng ghi dữ liệu, sao lưu `.git`, xác định object bị thiếu, tìm bản sao từ Remote, Clone khác, CI cache hoặc máy đồng đội. Không chạy GC hoặc thao tác phá hủy trước khi có bản sao. Sau khi phục hồi, chạy lại `git fsck`.

### Interviewer Notes

Đánh giá quy trình incident response hơn là nhớ command.

### Evaluation Rubric

- Senior: biết fsck.
- Staff: ưu tiên bảo toàn dữ liệu, nguồn phục hồi và xác minh toàn vẹn.
