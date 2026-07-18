# Inspect Status & History

> Kiểm tra trạng thái hiện tại của Repository, theo dõi các thay đổi và xem lịch sử Commit.

---

# Tổng quan

Trong quá trình làm việc với Git, trước khi thực hiện các thao tác như `add`, `commit`, `merge` hay `push`, bạn nên kiểm tra trạng thái hiện tại của Repository.

Bài viết này giới thiệu các lệnh giúp bạn:

- Kiểm tra trạng thái của Repository.
- Xem các thay đổi trong mã nguồn.
- So sánh sự khác biệt giữa các phiên bản.
- Xem lịch sử Commit.
- Theo dõi lịch sử thay đổi của từng dòng mã.
- Khôi phục các Commit hoặc Branch đã mất thông qua `reflog`.

---

# Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git status` | Hiển thị trạng thái hiện tại của Repository |
| `git diff` | Xem các thay đổi chưa được đưa vào Staging |
| `git diff --staged` | Xem các thay đổi đã được đưa vào Staging |
| `git log` | Xem lịch sử Commit |
| `git log --oneline` | Hiển thị lịch sử Commit ở dạng rút gọn |
| `git log --graph` | Hiển thị lịch sử Commit dưới dạng cây Branch |
| `git show` | Hiển thị chi tiết một Commit |
| `git blame` | Xem lịch sử chỉnh sửa của từng dòng trong một tệp |
| `git reflog` | Xem lịch sử di chuyển của HEAD |

---

# Kiểm tra trạng thái Repository

## Mục đích

Hiển thị trạng thái hiện tại của Working Tree và Staging Area.

Lệnh này cho biết:

- Tệp nào đã được chỉnh sửa.
- Tệp nào đã được đưa vào Staging.
- Tệp nào chưa được Git theo dõi.
- Repository đang đứng trên Branch nào.
- Branch hiện tại có đồng bộ với Remote hay không.

## Cú pháp

```bash
git status
```

## Khi nào nên sử dụng

Nên thực hiện trước khi:

- `git add`
- `git commit`
- `git push`

> **Lưu ý:** `git status` chỉ đọc thông tin và không làm thay đổi Repository.

---

# Xem các thay đổi chưa đưa vào Staging

## Mục đích

So sánh sự khác biệt giữa **Working Tree** và **Staging Area**.

## Cú pháp

```bash
git diff
```

Lệnh này chỉ hiển thị những thay đổi chưa được `git add`.

---

# Xem các thay đổi đã đưa vào Staging

## Mục đích

Kiểm tra chính xác nội dung sẽ được đưa vào Commit tiếp theo.

## Cú pháp

```bash
git diff --staged
```

hoặc

```bash
git diff --cached
```

Đây là bước nên thực hiện trước mỗi lần `git commit`.

---

# Xem lịch sử Commit

## Mục đích

Hiển thị toàn bộ lịch sử Commit của Repository.

## Cú pháp

```bash
git log
```

Thông tin thường bao gồm:

- Commit Hash
- Tác giả
- Thời gian
- Commit Message

---

# Xem lịch sử Commit rút gọn

## Mục đích

Hiển thị lịch sử Commit ngắn gọn để dễ theo dõi.

## Cú pháp

```bash
git log --oneline
```

Ví dụ:

```text
d34f9b2 Fix login bug
81bc31e Add authentication
c73d912 Initial commit
```

Đây là một trong những cách xem lịch sử Commit được sử dụng nhiều nhất.

---

# Hiển thị lịch sử Branch

## Mục đích

Hiển thị lịch sử Commit dưới dạng cây (Graph), giúp quan sát quá trình tạo Branch, Merge hoặc Rebase.

## Cú pháp

```bash
git log --graph --oneline --decorate --all
```

Phù hợp khi cần phân tích lịch sử phát triển của dự án.

---

# Xem chi tiết Commit

## Mục đích

Hiển thị toàn bộ thông tin của một Commit, bao gồm:

- Metadata
- Commit Message
- Nội dung thay đổi (Diff)

## Commit mới nhất

```bash
git show
```

## Commit cụ thể

```bash
git show <commit>
```

Ví dụ:

```bash
git show 8ab2f7d
```

---

# Xem lịch sử chỉnh sửa của tệp

## Mục đích

Xác định Commit và tác giả đã chỉnh sửa từng dòng trong một tệp.

## Cú pháp

```bash
git blame app.js
```

Thường sử dụng khi:

- Điều tra lỗi.
- Tìm người thực hiện thay đổi.
- Xác định Commit gây ra vấn đề.

> **Lưu ý:** `git blame` chỉ phục vụ mục đích phân tích lịch sử, không nên sử dụng để quy trách nhiệm cho cá nhân.

---

# Xem lịch sử của HEAD

## Mục đích

Hiển thị toàn bộ lịch sử di chuyển của HEAD.

## Cú pháp

```bash
git reflog
```

`git reflog` ghi lại mọi thay đổi của HEAD, kể cả khi Commit hoặc Branch không còn xuất hiện trong `git log`.

Đây là công cụ rất hữu ích để khôi phục dữ liệu.

## Một số trường hợp thường dùng

- Khôi phục Commit đã mất.
- Khôi phục Branch đã xóa.
- Hoàn tác sau khi `git reset --hard`.
- Tìm lại Commit sau khi Rebase hoặc Merge thất bại.

---

# Các lỗi thường gặp

## Commit mà không kiểm tra `git status`

Có thể vô tình Commit thiếu hoặc thừa tệp.

---

## Nhầm lẫn giữa `git diff` và `git diff --staged`

- `git diff` hiển thị thay đổi chưa đưa vào Staging.
- `git diff --staged` hiển thị thay đổi sẽ được Commit.

---

## Không sử dụng `git reflog` khi cần khôi phục

Nhiều người nghĩ Commit đã mất hoàn toàn sau khi `reset --hard`.

Thực tế, trong nhiều trường hợp, Commit vẫn có thể được tìm lại thông qua `git reflog`.

---

# Thực hành tốt

- Luôn chạy `git status` trước khi Commit hoặc Push.
- Kiểm tra nội dung thay đổi bằng `git diff` trước khi đưa vào Staging.
- Kiểm tra lại bằng `git diff --staged` trước khi tạo Commit.
- Sử dụng `git log --graph` để hiểu lịch sử phát triển của Branch.
- Làm quen với `git reflog` ngay từ đầu vì đây là công cụ hỗ trợ khôi phục dữ liệu rất mạnh của Git.

---

# Lệnh liên quan

- `git add`
- `git commit`
- `git restore`
- `git reset`
- `git checkout`

---

# Bài tiếp theo

→ [04. Stage & Commit](./04-stage-and-commit.md)