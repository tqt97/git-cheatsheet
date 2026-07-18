# Working Tree Interview Cases

## Case 01 - Bỏ thay đổi ở một tệp nhưng giữ các tệp khác

| Metadata | Value |
|---|---|
| Difficulty | 1/5 |
| Frequency | 5/5 |
| Mastery | 2/5 |
| Expected Level | Junior |
| Topics | Working Tree, restore, diff |

### Scenario

Bạn đã sửa ba tệp. Thay đổi trong `config.yml` là thử nghiệm và cần bỏ, nhưng thay đổi trong hai tệp còn lại phải được giữ nguyên.

### Questions for Candidate

Bạn sẽ kiểm tra và khôi phục như thế nào?

### Expected Analysis

Ứng viên cần phân biệt khôi phục một tệp trong Working Tree với Reset toàn bộ Repository.

### Possible Solutions

#### Solution A

```bash
git diff -- config.yml
git restore config.yml
```

Ưu điểm: rõ ràng, chỉ tác động một tệp.

Nhược điểm: thay đổi chưa Commit trong tệp sẽ bị mất.

#### Solution B

```bash
git checkout -- config.yml
```

Ưu điểm: hoạt động trên Git cũ.

Nhược điểm: cú pháp `checkout` đa nghĩa, khó đọc hơn `restore`.

### Recommended Answer

Dùng `git diff -- config.yml` để xác nhận nội dung sẽ mất, sau đó dùng `git restore config.yml`.

### Why

Lệnh chỉ cập nhật tệp trong Working Tree từ Index, không tác động các tệp khác.

### Interviewer Notes

Hỏi tiếp: nếu tệp đã được Stage thì sao?

### Evaluation Rubric

- Junior: biết `git restore`.
- Mid: kiểm tra `git diff` trước khi khôi phục.
- Senior: hỏi tệp đã Stage chưa và giải thích nguồn khôi phục mặc định là Index.

---

## Case 02 - Chuyển Branch khi Working Tree chưa sạch

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 5/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | Working Tree, switch, stash, commit |

### Scenario

Bạn đang sửa một Feature nhưng cần chuyển gấp sang `hotfix/payment`. Công việc hiện tại chưa đủ hoàn chỉnh để tạo Commit chính thức.

### Questions for Candidate

Bạn có những phương án nào và chọn phương án nào?

### Expected Analysis

Ứng viên cần cân nhắc Commit tạm, Stash hoặc tạo Worktree. Không nên mặc định Stash là phương án duy nhất.

### Possible Solutions

#### Solution A - Stash

```bash
git stash push -u -m "wip: current feature"
git switch hotfix/payment
```

Ưu điểm: nhanh, giữ Working Tree sạch.

Nhược điểm: Stash dễ bị quên, khó chia sẻ.

#### Solution B - WIP Commit

```bash
git add -A
git commit -m "wip: current feature"
git switch hotfix/payment
```

Ưu điểm: thay đổi được lưu trong lịch sử Branch, dễ khôi phục.

Nhược điểm: cần squash hoặc sửa lịch sử trước khi Merge.

#### Solution C - Worktree

```bash
git worktree add ../payment-hotfix hotfix/payment
```

Ưu điểm: làm song song, không cần cất thay đổi hiện tại.

Nhược điểm: yêu cầu hiểu Worktree và quản lý thêm thư mục.

### Recommended Answer

Nếu Hotfix ngắn và thay đổi hiện tại nhỏ, dùng Stash có thông điệp rõ ràng. Nếu công việc kéo dài hoặc quan trọng, ưu tiên WIP Commit. Nếu thường xuyên xử lý song song, dùng Worktree.

### Why

Lựa chọn phụ thuộc thời gian gián đoạn, khả năng cần chia sẻ và rủi ro quên trạng thái tạm.

### Interviewer Notes

Hỏi tiếp: tại sao `git stash -u` có thể cần thiết?

### Evaluation Rubric

- Junior: biết Stash.
- Mid: so sánh Stash và WIP Commit.
- Senior: đề xuất Worktree và xác định điều kiện dùng từng cách.

---

## Case 03 - Tệp generated liên tục xuất hiện trong status

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 4/5 |
| Mastery | 2/5 |
| Expected Level | Mid |
| Topics | .gitignore, tracked file, rm --cached |

### Scenario

Thư mục `dist/` đã từng được Commit. Sau khi thêm `dist/` vào `.gitignore`, các thay đổi trong thư mục vẫn xuất hiện trong `git status`.

### Questions for Candidate

Vì sao và xử lý thế nào?

### Expected Analysis

`.gitignore` chỉ áp dụng cho tệp chưa được Track.

### Possible Solutions

```bash
git rm -r --cached dist/
git commit -m "chore: stop tracking generated files"
```

Hoặc giữ các tệp trong Repository nếu chúng là artifact cần phát hành.

### Recommended Answer

Xác nhận `dist/` thực sự không nên được Track. Nếu đúng, dùng `git rm -r --cached dist/`, Commit thay đổi và giữ rule trong `.gitignore`.

### Why

Index vẫn chứa các tệp đã Track; `.gitignore` không tự xóa chúng khỏi Index.

### Interviewer Notes

Hỏi tiếp: tại sao không dùng `git rm -r dist/`?

### Evaluation Rubric

- Junior: biết thêm `.gitignore`.
- Mid: hiểu tracked và untracked.
- Senior: hỏi artifact có phải một phần quy trình Release không.

---

## Case 04 - Muốn xem chính xác thay đổi trước khi Commit

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 5/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | diff, staged diff, patch staging |

### Scenario

Một tệp chứa cả thay đổi sửa lỗi và Refactor. Bạn chỉ muốn Commit phần sửa lỗi.

### Questions for Candidate

Bạn sẽ chuẩn bị Commit như thế nào?

### Expected Analysis

Ứng viên nên biết Stage theo hunk, kiểm tra diff trước và sau Stage.

### Possible Solutions

```bash
git diff
git add -p path/to/file
git diff --staged
git commit -m "fix: handle invalid input"
```

Hoặc tách chỉnh sửa thủ công rồi Stage từng phần.

### Recommended Answer

Dùng `git add -p`, review bằng `git diff --staged`, rồi Commit phần sửa lỗi.

### Why

Index cho phép tạo Snapshot khác với toàn bộ trạng thái Working Tree.

### Interviewer Notes

Hỏi tiếp: làm sao sửa một hunk trước khi Stage?

### Evaluation Rubric

- Junior: Stage toàn bộ tệp.
- Mid: biết `git add -p`.
- Senior: giải thích Index là vùng xây dựng Commit và biết split/edit hunk.
