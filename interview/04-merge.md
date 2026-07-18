# Merge Interview Cases

## Case 01 - Fast-forward hay Merge Commit

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 4/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | fast-forward, merge commit, history |

### Scenario

Feature Branch đi thẳng từ `main`, không có thay đổi mới trên `main`. Bạn Merge Feature vào `main`.

### Possible Solutions

```bash
git merge feature
```

hoặc:

```bash
git merge --no-ff feature
```

### Recommended Answer

Theo policy của nhóm. Fast-forward giữ lịch sử gọn; `--no-ff` giữ ranh giới Feature bằng Merge Commit. Ứng viên cần giải thích mục tiêu lịch sử thay vì chọn máy móc.

### Interviewer Notes

Hỏi tiếp: Fast-forward có tạo Commit mới không?

### Evaluation Rubric

- Junior: biết Merge.
- Mid: hiểu Fast-forward.
- Senior: liên hệ chiến lược Release và Revert Feature.

---

## Case 02 - Revert một Merge Commit

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 3/5 |
| Mastery | 5/5 |
| Expected Level | Senior |
| Topics | merge parent, revert -m, history |

### Scenario

Một Feature đã Merge vào `main` bằng Merge Commit và gây lỗi Production. Branch đã được chia sẻ rộng rãi.

### Possible Solutions

```bash
git revert -m 1 <merge-commit>
```

Hoặc Revert từng Commit của Feature.

### Recommended Answer

Dùng `git revert -m 1` nếu parent 1 là dòng lịch sử chính và toàn bộ Feature cần đảo ngược. Trước khi chạy phải kiểm tra parent order bằng `git show`.

### Why

Git cần biết parent nào được coi là mainline để tính phần thay đổi cần đảo ngược.

### Interviewer Notes

Hỏi tiếp: điều gì xảy ra khi Merge lại cùng Feature sau đó?

### Evaluation Rubric

- Mid: biết Revert Merge.
- Senior: hiểu `-m` là mainline parent.
- Staff: dự đoán ảnh hưởng đến Merge tương lai và kế hoạch Revert the Revert.

---

## Case 03 - Octopus Merge trong Release

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 1/5 |
| Mastery | 4/5 |
| Expected Level | Staff |
| Topics | octopus merge, release strategy |

### Scenario

Một người đề xuất Merge cùng lúc bốn Feature Branch vào Release Branch bằng một Octopus Merge.

### Possible Solutions

```bash
git merge feature/a feature/b feature/c feature/d
```

Hoặc Merge từng Branch riêng qua Pull Request.

### Recommended Answer

Trong quy trình Review thông thường, Merge từng Branch riêng để giữ khả năng kiểm tra, rollback và xác định lỗi. Octopus Merge phù hợp hơn với các Branch độc lập, ít xung đột và quy trình tích hợp đặc biệt.

### Interviewer Notes

Hỏi ứng viên đánh đổi giữa lịch sử gọn và khả năng audit.

### Evaluation Rubric

- Senior: biết Octopus Merge tồn tại.
- Staff: từ chối dùng chỉ vì kỹ thuật cho phép và phân tích vận hành.

---

## Case 04 - Merge nhầm Branch vào local main

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 3/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | reset, reflog, shared history |

### Scenario

Bạn Merge nhầm `feature/experimental` vào local `main`. Merge chưa Push và Working Tree sạch.

### Possible Solutions

```bash
git reset --hard ORIG_HEAD
```

Hoặc dùng Reflog xác định Commit trước Merge.

### Recommended Answer

Nếu chưa Push và không có thay đổi cần giữ, Reset về `ORIG_HEAD`. Nếu trạng thái phức tạp, kiểm tra Reflog trước rồi Reset đến Commit chính xác.

### Interviewer Notes

Hỏi tiếp: nếu Merge đã Push thì sao?

### Evaluation Rubric

- Mid: biết Reset về Commit trước.
- Senior: biết `ORIG_HEAD` và kiểm tra chia sẻ.
- Staff: chọn Revert nếu lịch sử đã công khai.
