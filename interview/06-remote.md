# Remote Interview Cases

## Case 01 - Push bị từ chối Non-fast-forward

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 5/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | fetch, pull, non-fast-forward |

### Scenario

`git push` bị từ chối vì Remote Branch có Commit mới.

### Possible Solutions

```bash
git fetch origin
git rebase origin/<branch>
```

hoặc:

```bash
git pull --no-rebase
```

### Recommended Answer

Fetch trước để quan sát Divergence. Rebase nếu Branch cá nhân; Merge nếu Branch chia sẻ hoặc team policy yêu cầu.

### Interviewer Notes

Không chấp nhận câu trả lời "force push" nếu chưa phân tích.

### Evaluation Rubric

- Junior: biết Pull.
- Mid: Fetch rồi quan sát.
- Senior: chọn Merge/Rebase theo ownership.

---

## Case 02 - origin/main không cập nhật sau khi đồng đội Push

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 4/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | remote-tracking branch, fetch |

### Scenario

Đồng đội nói `main` đã có Commit mới nhưng `git log origin/main` trên máy bạn chưa thấy.

### Recommended Answer

Chạy `git fetch origin`, sau đó kiểm tra lại. Giải thích `origin/main` là Remote Tracking Branch cục bộ, chỉ cập nhật khi Fetch hoặc Pull.

### Interviewer Notes

Hỏi tiếp: `origin/main` có phải Branch nằm trên server không?

### Evaluation Rubric

- Junior: biết Fetch.
- Mid: hiểu Remote Tracking Branch là Reference cục bộ.
- Senior: giải thích FETCH_HEAD và refspec ở mức phù hợp.

---

## Case 03 - Đổi URL Remote từ HTTPS sang SSH

| Metadata | Value |
|---|---|
| Difficulty | 1/5 |
| Frequency | 3/5 |
| Mastery | 2/5 |
| Expected Level | Junior |
| Topics | remote config |

### Possible Solutions

```bash
git remote set-url origin git@host:org/repo.git
git remote -v
```

### Recommended Answer

Cập nhật URL, kiểm tra fetch/push URL và thử kết nối. Không cần Clone lại Repository.

### Interviewer Notes

Hỏi ứng viên fetch URL và push URL có thể khác nhau không.

### Evaluation Rubric

- Junior: biết `set-url`.
- Senior: biết cấu hình URL riêng cho push trong trường hợp đặc biệt.

---

## Case 04 - Xóa Remote Branch nhưng local vẫn hiển thị

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 4/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | prune, remote-tracking refs |

### Scenario

Remote Branch đã bị xóa trên server nhưng `git branch -r` vẫn hiển thị.

### Possible Solutions

```bash
git fetch --prune
```

hoặc:

```bash
git remote prune origin
```

### Recommended Answer

Dùng `git fetch --prune` để vừa cập nhật vừa xóa Remote Tracking References đã stale.

### Why

Xóa Branch trên server không tự động cập nhật Reference cục bộ.

### Interviewer Notes

Hỏi tiếp: lệnh này có xóa Local Branch không?

### Evaluation Rubric

- Mid: biết Prune.
- Senior: phân biệt Local Branch và Remote Tracking Branch.
