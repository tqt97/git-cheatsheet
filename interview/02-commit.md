# Commit Interview Cases

## Case 01 - Quên thêm một tệp vào Commit cuối

| Metadata | Value |
|---|---|
| Difficulty | 1/5 |
| Frequency | 5/5 |
| Mastery | 2/5 |
| Expected Level | Junior |
| Topics | amend, Index, Commit |

### Scenario

Bạn vừa tạo Commit nhưng quên Stage một tệp liên quan. Commit chưa được Push.

### Possible Solutions

#### Solution A

```bash
git add missing-file
git commit --amend --no-edit
```

#### Solution B

Tạo Commit mới cho tệp bị thiếu.

### Recommended Answer

Dùng Amend nếu Commit chưa được chia sẻ và tệp thực sự thuộc cùng một thay đổi logic. Nếu Commit đã Push hoặc cần giữ lịch sử rõ ràng, tạo Commit mới.

### Why

Amend tạo Commit mới với hash mới; nó không sửa Commit cũ tại chỗ.

### Interviewer Notes

Hỏi tiếp: điều gì xảy ra nếu Commit đã Push?

### Evaluation Rubric

- Junior: biết Amend.
- Mid: biết Amend thay đổi hash.
- Senior: quyết định theo trạng thái chia sẻ của Branch.

---

## Case 02 - Commit chứa hai thay đổi không liên quan

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | reset, patch staging, commit design |

### Scenario

Commit cuối chứa một Bug Fix và một Refactor lớn. Commit chưa Push. Reviewer yêu cầu tách thành hai Commit.

### Possible Solutions

```bash
git reset HEAD~1
git add -p
git commit -m "fix: ..."
git add -A
git commit -m "refactor: ..."
```

Hoặc dùng Interactive Rebase nếu thay đổi nằm trong nhiều Commit.

### Recommended Answer

Với Commit cuối chưa Push, dùng Mixed Reset để đưa thay đổi về Working Tree, sau đó Stage theo hunk và tạo lại hai Commit.

### Why

Mixed Reset di chuyển Branch nhưng giữ nội dung Working Tree, phù hợp để tái cấu trúc Commit.

### Interviewer Notes

Hỏi tiếp: khác gì với `reset --soft`?

### Evaluation Rubric

- Mid: biết Reset Commit cuối.
- Senior: chọn đúng mode và tách theo ý nghĩa nghiệp vụ.
- Staff: nói về atomic Commit và khả năng Revert độc lập.

---

## Case 03 - Commit message sai sau khi đã Push

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 3/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | amend, force-with-lease, shared history |

### Scenario

Commit cuối đã Push lên Feature Branch cá nhân nhưng chưa có ai dựa trên Branch này. Message sai nghiêm trọng và cần sửa trước Pull Request.

### Possible Solutions

#### Solution A

```bash
git commit --amend -m "correct message"
git push --force-with-lease
```

#### Solution B

Giữ nguyên và giải thích trong Pull Request.

### Recommended Answer

Nếu xác nhận Branch không được chia sẻ, Amend và Push bằng `--force-with-lease`. Nếu có khả năng người khác đã Fetch hoặc làm việc trên Branch, tránh Rewrite hoặc phối hợp trước.

### Why

`--force-with-lease` kiểm tra Remote Reference chưa thay đổi ngoài dự kiến, nhưng không loại bỏ rủi ro cộng tác.

### Interviewer Notes

Hỏi tiếp: `--force-with-lease` bảo vệ được điều gì và không bảo vệ được điều gì?

### Evaluation Rubric

- Mid: biết Force Push.
- Senior: dùng `--force-with-lease`.
- Staff: xác minh ownership và thông báo tác động trước khi Rewrite.

---

## Case 04 - Tìm Commit gây ra Regression

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | bisect, history, testing |

### Scenario

Regression xuất hiện đâu đó trong khoảng 200 Commit gần nhất. Bạn có một test script xác định trạng thái tốt hoặc xấu.

### Possible Solutions

```bash
git bisect start
git bisect bad HEAD
git bisect good <known-good-commit>
git bisect run ./test-regression.sh
git bisect reset
```

Hoặc kiểm tra thủ công từng Commit.

### Recommended Answer

Dùng `git bisect` kết hợp test tự động.

### Why

Binary Search giảm số lần kiểm tra từ tuyến tính xuống logarit.

### Interviewer Notes

Hỏi tiếp: test không ổn định ảnh hưởng thế nào đến Bisect?

### Evaluation Rubric

- Mid: biết xem Log.
- Senior: biết Bisect.
- Staff: quan tâm tính xác định của test và Commit không build được.
