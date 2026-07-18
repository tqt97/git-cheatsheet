# Undo & Recovery

> Hoàn tác các thay đổi và khôi phục dữ liệu trong Git một cách an toàn.

---

## Tổng quan

Trong quá trình làm việc với Git, việc thao tác nhầm là điều khó tránh khỏi:

- Chỉnh sửa nhầm một tệp.
- `git add` nhầm tệp.
- Commit sai.
- Xóa nhầm Branch.
- `git reset --hard`.
- Mất Commit sau khi Rebase.

Git cung cấp nhiều lệnh để hoàn tác hoặc khôi phục dữ liệu. Tuy nhiên, mỗi lệnh có phạm vi tác động khác nhau.

Hiểu rõ sự khác biệt giữa các lệnh này sẽ giúp bạn tránh mất dữ liệu và lựa chọn đúng công cụ cho từng tình huống.

---

## Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git restore` | Khôi phục nội dung tệp |
| `git restore --staged` | Gỡ tệp khỏi Staging Area |
| `git reset --soft` | Hoàn tác Commit nhưng giữ Staging |
| `git reset --mixed` | Hoàn tác Commit và Staging |
| `git reset --hard` | Đưa Repository về trạng thái của một Commit |
| `git revert` | Tạo Commit mới để hoàn tác Commit cũ |
| `git reflog` | Khôi phục Commit hoặc Branch đã mất |

---

## git restore

### Mục đích

Khôi phục nội dung của một hoặc nhiều tệp về trạng thái ở Commit gần nhất.

Các thay đổi trong Working Tree sẽ bị loại bỏ.

### Cú pháp

```bash
git restore <file>
```

Ví dụ

```bash
git restore README.md
```

---

## Khôi phục toàn bộ Working Tree

```bash
git restore .
```

Tất cả thay đổi chưa được Staging sẽ bị hủy.

> **Lưu ý:** Không thể hoàn tác thao tác này nếu thay đổi chưa từng được Commit.

---

## git restore --staged

### Mục đích

Gỡ tệp khỏi Staging Area nhưng vẫn giữ nguyên nội dung đã chỉnh sửa.

### Cú pháp

```bash
git restore --staged <file>
```

Ví dụ

```bash
git restore --staged README.md
```

Phù hợp khi:

- `git add` nhầm tệp.
- Muốn chia nhỏ Commit.

---

## git reset

`git reset` thay đổi vị trí của HEAD.

Có ba chế độ phổ biến:

- `--soft`
- `--mixed`
- `--hard`

---

## git reset --soft

### Mục đích

Hoàn tác Commit gần nhất nhưng vẫn giữ các thay đổi trong Staging Area.

### Cú pháp

```bash
git reset --soft HEAD~1
```

Phù hợp khi:

- Muốn sửa Commit Message.
- Muốn gộp nhiều Commit.

---

## git reset --mixed

### Mục đích

Hoàn tác Commit và đưa các thay đổi trở lại Working Tree.

Đây là chế độ mặc định của `git reset`.

### Cú pháp

```bash
git reset HEAD~1
```

hoặc

```bash
git reset --mixed HEAD~1
```

Sau khi thực hiện:

- Commit bị xóa.
- Staging được làm sạch.
- Nội dung tệp vẫn được giữ lại.

---

## git reset --hard

### Mục đích

Đưa Repository về đúng trạng thái của một Commit.

Mọi thay đổi sau Commit đó sẽ bị loại bỏ.

### Cú pháp

```bash
git reset --hard HEAD~1
```

Ví dụ

```bash
git reset --hard a13b7d2
```

> **Cảnh báo:** `git reset --hard` sẽ xóa toàn bộ thay đổi chưa được Commit. Hãy chắc chắn rằng bạn không còn cần các thay đổi đó trước khi thực hiện.

---

## So sánh các chế độ của git reset

| Chế độ | Commit | Staging | Working Tree |
|---------|--------|---------|--------------|
| `--soft` | Hoàn tác | Giữ nguyên | Giữ nguyên |
| `--mixed` | Hoàn tác | Hoàn tác | Giữ nguyên |
| `--hard` | Hoàn tác | Hoàn tác | Hoàn tác |

---

## git revert

### Mục đích

Hoàn tác một Commit bằng cách tạo ra một Commit mới có nội dung ngược lại.

Không làm thay đổi lịch sử Commit.

### Cú pháp

```bash
git revert <commit>
```

Ví dụ

```bash
git revert a13b7d2
```

Git sẽ tạo thêm một Commit mới.

Ví dụ

```text
A --- B --- C
          │
          ▼
git revert C
          │
          ▼
A --- B --- C --- D
```

Trong đó:

```
D
```

là Commit hoàn tác.

---

## Khi nào dùng git revert?

Nên sử dụng khi:

- Commit đã được Push lên Remote.
- Repository có nhiều người cùng làm việc.
- Không muốn thay đổi lịch sử Commit.

---

## git reflog

### Mục đích

Hiển thị lịch sử di chuyển của HEAD.

Khác với `git log`, `git reflog` vẫn lưu lại các Commit ngay cả khi chúng không còn xuất hiện trong lịch sử hiện tại.

### Cú pháp

```bash
git reflog
```

Ví dụ

```text
8ab2f7d HEAD@{0}: reset: moving to HEAD~1

f41d1ce HEAD@{1}: commit: Add login

b312ac0 HEAD@{2}: commit: Initial project
```

---

## Khôi phục Commit đã mất

Giả sử vừa thực hiện:

```bash
git reset --hard HEAD~1
```

Sau đó chạy:

```bash
git reflog
```

Lấy lại Commit

```bash
git reset --hard f41d1ce
```

Repository sẽ trở về đúng trạng thái trước khi Reset.

---

## Khôi phục Branch đã xóa

Giả sử Branch đã bị xóa:

```bash
git branch -D feature/login
```

Tìm Commit cuối cùng bằng:

```bash
git reflog
```

Tạo lại Branch:

```bash
git branch feature/login <commit>
```

---

## Sơ đồ lựa chọn

```text
Muốn hoàn tác?

        │
        ▼

Chỉ bỏ thay đổi trong file?

        │
        ├──► git restore

        ▼

Đã add nhầm?

        │
        ├──► git restore --staged

        ▼

Commit chưa Push?

        │
        ├──► git reset

        ▼

Commit đã Push?

        │
        ├──► git revert

        ▼

Mất Commit?

        │
        └──► git reflog
```

---

## Các lỗi thường gặp

### Sử dụng `git reset --hard` quá sớm

Đây là nguyên nhân phổ biến nhất dẫn đến mất dữ liệu.

Nếu chưa chắc chắn, hãy sử dụng:

```bash
git restore
```

hoặc

```bash
git reset --soft
```

---

### Dùng `git reset` cho Commit đã Push

Việc thay đổi lịch sử của Commit đã chia sẻ có thể gây xung đột với các thành viên khác.

Trong trường hợp này nên sử dụng:

```bash
git revert
```

---

### Không biết đến `git reflog`

Nhiều người nghĩ dữ liệu đã mất hoàn toàn sau khi:

- Reset.
- Rebase.
- Xóa Branch.

Trong nhiều trường hợp, `git reflog` vẫn có thể giúp khôi phục.

---

## Thực hành tốt

- Luôn kiểm tra `git status` trước khi hoàn tác.
- Ưu tiên `git restore` khi chỉ muốn hủy thay đổi của tệp.
- Chỉ sử dụng `git reset --hard` khi hiểu rõ hậu quả.
- Sử dụng `git revert` cho các Commit đã được Push.
- Ghi nhớ `git reflog` là công cụ quan trọng nhất để khôi phục dữ liệu trong Git.

---

## Lệnh liên quan

- `git status`
- `git diff`
- `git add`
- `git commit`
- `git cherry-pick`

---

## Bài tiếp theo

→ [09. Stash & Cherry-pick](./09-stash-and-cherry-pick.md)