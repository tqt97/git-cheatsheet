# Sync With Remote

> Đồng bộ dữ liệu giữa Local Repository và Remote Repository.

---

## Tổng quan

Trong quá trình phát triển phần mềm, Repository trên máy tính (Local Repository) cần được đồng bộ với Repository trên máy chủ (Remote Repository).

Git cung cấp các lệnh để:

- Tải thông tin mới từ Remote.
- Cập nhật Branch hiện tại.
- Đẩy Commit lên Remote.
- Thiết lập Branch theo dõi (Tracking Branch).

Hiểu đúng các lệnh này sẽ giúp hạn chế xung đột và tránh ghi đè lịch sử làm việc của người khác.

---

## Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git fetch` | Tải thông tin mới từ Remote |
| `git pull` | Tải và cập nhật Branch hiện tại |
| `git pull --rebase` | Cập nhật bằng Rebase |
| `git push` | Đẩy Commit lên Remote |
| `git push -u` | Thiết lập Tracking Branch |
| `git push --force-with-lease` | Ghi đè lịch sử một cách an toàn hơn |

---

## Tải dữ liệu từ Remote

### Mục đích

Tải Commit, Branch và Tag mới từ Remote nhưng **không thay đổi Branch hiện tại**.

### Cú pháp

```bash
git fetch
```

Sau khi `fetch`, bạn có thể kiểm tra thay đổi trước khi Merge hoặc Rebase.

---

## Cập nhật Branch hiện tại

### Mục đích

Lấy dữ liệu mới từ Remote và cập nhật Branch hiện tại.

### Cú pháp

```bash
git pull
```

Thực chất:

```text
git fetch
        +
git merge
```

---

## Cập nhật bằng Rebase

### Mục đích

Lấy thay đổi mới từ Remote nhưng giữ lịch sử Commit tuyến tính.

### Cú pháp

```bash
git pull --rebase
```

Thực chất:

```text
git fetch
        +
git rebase
```

Thường được sử dụng trong các dự án muốn giữ lịch sử Commit gọn gàng.

---

## Đẩy Commit lên Remote

### Mục đích

Đồng bộ các Commit từ Local Repository lên Remote Repository.

### Cú pháp

```bash
git push
```

Ví dụ:

```bash
git push origin feature/login
```

---

## Thiết lập Tracking Branch

### Mục đích

Liên kết Local Branch với Remote Branch.

Sau lần đầu thiết lập, chỉ cần sử dụng:

```bash
git push
```

hoặc

```bash
git pull
```

### Cú pháp

```bash
git push -u origin feature/login
```

---

## Ghi đè lịch sử an toàn hơn

### Mục đích

Đẩy lịch sử mới lên Remote sau khi đã Rebase hoặc chỉnh sửa Commit.

### Cú pháp

```bash
git push --force-with-lease
```

Khác với:

```bash
git push --force
```

`--force-with-lease` sẽ từ chối ghi đè nếu Remote đã có thay đổi mới từ người khác.

Đây là lựa chọn được khuyến nghị khi cần Force Push.

---

## Quy trình làm việc

```text
Remote Repository
        │
        ▼
   git fetch
        │
        ▼
Kiểm tra thay đổi
        │
        ▼
git pull --rebase
        │
        ▼
Phát triển tính năng
        │
        ▼
git push
```

---

## Các lỗi thường gặp

### Push khi chưa Pull

Có thể dẫn đến lỗi:

```text
rejected (non-fast-forward)
```

---

### Lạm dụng `git push --force`

Có thể ghi đè Commit của đồng đội.

Nếu bắt buộc phải Force Push, nên sử dụng:

```bash
git push --force-with-lease
```

---

### Không thiết lập Tracking Branch

Khiến mỗi lần Push phải chỉ định đầy đủ:

```bash
git push origin feature/login
```

---

### Nhầm lẫn giữa `fetch` và `pull`

- `fetch` chỉ tải dữ liệu.
- `pull` tải dữ liệu và cập nhật Branch hiện tại.

---

## Thực hành tốt

- Luôn `git fetch` trước khi Merge hoặc Rebase.
- Ưu tiên `git pull --rebase` nếu nhóm sử dụng lịch sử tuyến tính.
- Chỉ Force Push khi thực sự cần thiết.
- Luôn sử dụng `--force-with-lease` thay cho `--force`.
- Push thường xuyên để giảm nguy cơ xung đột.

---

## Lệnh liên quan

- `git remote`
- `git merge`
- `git rebase`
- `git branch`

---

## Bài tiếp theo

→ [07. Merge & Rebase](./07-merge-and-rebase.md)