# Git Internals Interview Cases

## Case 01 - Branch thực sự là gì

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Mid |
| Topics | refs, commits, branch pointer |

### Scenario

Ứng viên được hỏi điều gì xảy ra trong `.git` khi tạo Branch mới.

### Recommended Answer

Git tạo một Reference mới trỏ đến Commit hiện tại. Không sao chép Working Tree hoặc toàn bộ lịch sử.

Có thể kiểm chứng bằng:

```bash
git branch feature
git rev-parse feature
git rev-parse HEAD
```

### Evaluation Rubric

- Junior: Branch là nhánh phát triển.
- Mid: Branch là pointer.
- Senior: mô tả ref storage và packed-refs.

---

## Case 02 - Tại sao sửa Commit làm đổi hash

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 3/5 |
| Mastery | 5/5 |
| Expected Level | Senior |
| Topics | commit object, parent, tree, metadata, hash |

### Recommended Answer

Commit hash phụ thuộc nội dung Commit object, gồm Tree, Parent, Author, Committer, timestamp và message. Thay đổi bất kỳ phần nào tạo Object mới và hash mới. Descendant Commit cũng cần được tạo lại vì parent hash thay đổi.

### Interviewer Notes

Hỏi tại sao Rebase thay đổi hash dù file cuối cùng giống nhau.

### Evaluation Rubric

- Mid: biết hash phụ thuộc nội dung.
- Senior: liệt kê thành phần Commit object.
- Staff: giải thích hiệu ứng lan truyền trên DAG.

---

## Case 03 - Git lưu Snapshot hay Diff

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 3/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | blob, tree, snapshot, delta compression |

### Recommended Answer

Mô hình logic của Git là Snapshot qua Tree và Blob. Ở tầng lưu trữ packfile, Git có thể dùng delta compression để tiết kiệm dung lượng. Không nên nhầm implementation optimization với data model.

### Interviewer Notes

Đây là câu phân biệt mental model và storage optimization.

### Evaluation Rubric

- Mid: nói Git lưu Snapshot.
- Senior: phân biệt object model với packfile delta.
- Staff: giải thích vì sao cả hai mô tả có thể đúng ở hai tầng khác nhau.

---

## Case 04 - Commit không còn Branch nào trỏ tới

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 2/5 |
| Mastery | 5/5 |
| Expected Level | Senior |
| Topics | reachability, reflog, gc |

### Scenario

Sau Reset, Commit không còn nằm trên Branch nào. Commit đã bị xóa chưa?

### Recommended Answer

Chưa chắc. Commit trở thành unreachable từ các Reference chính nhưng có thể còn reachable qua Reflog. Object chỉ có thể bị dọn sau khi hết thời hạn bảo vệ và Garbage Collection xử lý.

### Interviewer Notes

Hỏi ứng viên phân biệt unreachable và deleted.

### Evaluation Rubric

- Mid: biết Reflog cứu được Commit.
- Senior: hiểu Reachability.
- Staff: mô tả Reflog expiry và GC ở mức khái niệm.

---

## Case 05 - Tạo Commit bằng Plumbing Commands

| Metadata | Value |
|---|---|
| Difficulty | 5/5 |
| Frequency | 1/5 |
| Mastery | 5/5 |
| Expected Level | Staff |
| Topics | hash-object, update-index, write-tree, commit-tree, update-ref |

### Scenario

Ứng viên cần mô tả các bước tối thiểu Git thực hiện phía sau `git add` và `git commit`.

### Recommended Answer

Một quy trình đơn giản có thể gồm:

1. Ghi Blob bằng `git hash-object -w`.
2. Cập nhật Index bằng `git update-index`.
3. Tạo Tree bằng `git write-tree`.
4. Tạo Commit bằng `git commit-tree`.
5. Di chuyển Branch bằng `git update-ref`.

### Interviewer Notes

Không yêu cầu thuộc chính xác cú pháp; đánh giá mental model về Object và Reference.

### Evaluation Rubric

- Senior: biết Blob, Tree, Commit.
- Staff: nối đúng Object creation với Reference movement.
