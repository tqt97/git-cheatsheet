# Branch & Switch

> Tạo, quản lý và chuyển đổi giữa các Branch trong Git.

---

## Tổng quan

Branch là một trong những tính năng quan trọng nhất của Git.

Thay vì làm việc trực tiếp trên nhánh chính (`main`), bạn nên tạo Branch mới cho từng tính năng, bản sửa lỗi hoặc thử nghiệm.

Bài viết này giới thiệu các lệnh giúp bạn:

- Xem danh sách Branch.
- Tạo Branch mới.
- Chuyển đổi giữa các Branch.
- Đổi tên Branch.
- Xóa Branch không còn sử dụng.

---

## Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git branch` | Hiển thị danh sách Branch |
| `git branch <branch>` | Tạo Branch mới |
| `git branch -a` | Hiển thị tất cả Local và Remote Branch |
| `git branch -d` | Xóa Branch đã Merge |
| `git branch -D` | Buộc xóa Branch |
| `git branch -m` | Đổi tên Branch |
| `git switch` | Chuyển sang Branch khác |
| `git switch -c` | Tạo và chuyển sang Branch mới |
| `git checkout` | Chuyển Branch (cách cũ) |
| `git checkout -b` | Tạo và chuyển Branch (cách cũ) |

---

## Xem danh sách Branch

### Mục đích

Hiển thị các Branch hiện có trong Repository.

### Cú pháp

```bash
git branch
```

Ví dụ:

```text
* main
  develop
  feature/login
```

Dấu `*` cho biết Branch đang làm việc.

---

## Xem tất cả Branch

### Mục đích

Hiển thị cả Local Branch và Remote Branch.

### Cú pháp

```bash
git branch -a
```

Ví dụ:

```text
* main
  develop
  remotes/origin/main
  remotes/origin/develop
```

---

## Tạo Branch mới

### Mục đích

Tạo một Branch mới từ Commit hiện tại.

### Cú pháp

```bash
git branch feature/login
```

Sau khi tạo, Git vẫn giữ bạn ở Branch hiện tại.

---

## Chuyển sang Branch khác

### Mục đích

Chuyển Working Tree sang một Branch đã tồn tại.

### Cú pháp

```bash
git switch develop
```

Ví dụ:

```bash
git switch feature/login
```

---

## Tạo và chuyển sang Branch mới

### Mục đích

Tạo Branch mới và chuyển sang Branch đó ngay lập tức.

### Cú pháp

```bash
git switch -c feature/login
```

Lệnh này tương đương:

```bash
git branch feature/login
git switch feature/login
```

---

## Đổi tên Branch

### Mục đích

Đổi tên Branch hiện tại hoặc một Branch khác.

### Cú pháp

Đổi tên Branch hiện tại:

```bash
git branch -m new-name
```

Đổi tên Branch khác:

```bash
git branch -m old-name new-name
```

---

## Xóa Branch

### Xóa Branch đã Merge

```bash
git branch -d feature/login
```

Git sẽ từ chối nếu Branch vẫn còn Commit chưa được Merge.

---

### Buộc xóa Branch

```bash
git branch -D feature/login
```

Sử dụng khi chắc chắn không cần giữ lịch sử trên Branch đó.

---

## git checkout

Trước Git 2.23, `git checkout` được dùng để chuyển Branch.

```bash
git checkout develop
```

Hoặc tạo Branch mới:

```bash
git checkout -b feature/login
```

Ngày nay, Git khuyến nghị sử dụng:

- `git switch` để chuyển Branch.
- `git restore` để khôi phục tệp.

Điều này giúp câu lệnh rõ ràng và dễ hiểu hơn.

---

## Quy trình làm việc

```text
main
  │
  ├───────────────┐
  ▼               │
git switch -c feature/login
                  │
                  ▼
           Phát triển tính năng
                  │
                  ▼
            Commit thay đổi
                  │
                  ▼
              Merge vào main
                  │
                  ▼
       git branch -d feature/login
```

---

## Các lỗi thường gặp

### Làm việc trực tiếp trên `main`

Dễ gây xung đột và khó quản lý lịch sử.

Nên tạo Branch riêng cho từng tính năng.

---

### Xóa nhầm Branch

Sử dụng:

```bash
git branch -D
```

có thể làm mất các Commit chưa Merge.

---

### Quên chuyển Branch

Nhiều người vô tình Commit vào sai Branch.

Luôn kiểm tra bằng:

```bash
git branch
```

hoặc

```bash
git status
```

---

### Đặt tên Branch không rõ ràng

Ví dụ:

```text
test
new
abc
fix
```

Nên sử dụng tên có ý nghĩa.

Ví dụ:

```text
feature/login
bugfix/authentication
hotfix/payment
release/v1.2.0
```

---

## Thực hành tốt

- Mỗi tính năng nên được phát triển trên một Branch riêng.
- Không Commit trực tiếp lên `main`.
- Đặt tên Branch theo quy ước của nhóm.
- Xóa Branch sau khi đã Merge để giữ Repository gọn gàng.
- Ưu tiên sử dụng `git switch` thay cho `git checkout`.

---

## Lệnh liên quan

- `git merge`
- `git rebase`
- `git fetch`
- `git push`

---

## Bài tiếp theo

→ [06. Sync With Remote](./06-sync-with-remote.md)