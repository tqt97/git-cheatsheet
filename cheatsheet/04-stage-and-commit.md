# Stage & Commit

> Chuẩn bị các thay đổi và tạo Commit để lưu lại lịch sử phát triển của Repository.

---

## Tổng quan

Trong Git, việc lưu thay đổi được thực hiện theo hai bước:

1. Đưa thay đổi vào **Staging Area**.
2. Tạo **Commit** để lưu các thay đổi đó vào lịch sử Repository.

Việc tách riêng hai bước giúp bạn kiểm soát chính xác những gì sẽ được đưa vào mỗi Commit.

---

## Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git add` | Đưa tệp vào Staging Area |
| `git add .` | Đưa các thay đổi trong thư mục hiện tại vào Staging |
| `git add -A` | Đưa toàn bộ thay đổi vào Staging |
| `git add -p` | Chọn từng phần thay đổi để đưa vào Staging |
| `git restore --staged` | Gỡ tệp khỏi Staging |
| `git commit` | Tạo Commit |
| `git commit -m` | Tạo Commit với thông điệp |
| `git commit --amend` | Chỉnh sửa Commit gần nhất |

---

## Đưa thay đổi vào Staging

### Mục đích

Đưa một hoặc nhiều tệp từ Working Tree vào Staging Area để chuẩn bị Commit.

### Cú pháp

```bash
git add <file>
```

Ví dụ

```bash
git add README.md
```

---

## Đưa toàn bộ thay đổi trong thư mục hiện tại vào Staging

```bash
git add .
```

Lệnh này chỉ áp dụng cho thư mục hiện tại và các thư mục con.

---

## Đưa toàn bộ thay đổi vào Staging

```bash
git add -A
```

Bao gồm:

- Tệp mới.
- Tệp đã chỉnh sửa.
- Tệp đã xóa.

Đây là lựa chọn phù hợp khi muốn Commit toàn bộ thay đổi trong Repository.

---

## Chọn từng phần thay đổi

### Mục đích

Cho phép lựa chọn từng đoạn mã (Hunk) để đưa vào Staging.

### Cú pháp

```bash
git add -p
```

Thích hợp khi:

- Một tệp chứa nhiều thay đổi khác nhau.
- Muốn tách thành nhiều Commit nhỏ.
- Giữ Commit rõ ràng và dễ theo dõi.

---

## Gỡ tệp khỏi Staging

### Mục đích

Đưa tệp ra khỏi Staging nhưng vẫn giữ nguyên nội dung đã chỉnh sửa.

### Cú pháp

```bash
git restore --staged <file>
```

Ví dụ

```bash
git restore --staged README.md
```

---

## Tạo Commit

### Mục đích

Lưu các thay đổi trong Staging Area thành một Commit mới.

### Cú pháp

```bash
git commit
```

Git sẽ mở trình soạn thảo để nhập Commit Message.

---

## Tạo Commit với Commit Message

```bash
git commit -m "Add login feature"
```

Ví dụ

```bash
git commit -m "Fix authentication bug"
```

---

## Chỉnh sửa Commit gần nhất

### Mục đích

Thay đổi Commit Message hoặc bổ sung thay đổi vào Commit vừa tạo.

### Cú pháp

```bash
git commit --amend
```

Nếu chỉ muốn sửa Commit Message

```bash
git commit --amend -m "New commit message"
```

> Không nên sử dụng `--amend` với Commit đã Push lên Remote nếu nhiều người đang cùng làm việc.

---

## Quy trình làm việc

```text
Chỉnh sửa mã nguồn
        │
        ▼
git status
        │
        ▼
git add
        │
        ▼
git diff --staged
        │
        ▼
git commit
```

---

## Các lỗi thường gặp

### Commit mà chưa kiểm tra Staging

Có thể bỏ sót hoặc Commit nhầm tệp.

Luôn kiểm tra bằng:

```bash
git diff --staged
```

---

### Commit quá nhiều nội dung

Một Commit chứa nhiều tính năng hoặc nhiều lỗi được sửa sẽ gây khó khăn khi:

- Review.
- Debug.
- Revert.

---

### Lạm dụng `git add .`

`git add .` có thể đưa cả những tệp không mong muốn vào Staging.

Nên kiểm tra bằng:

```bash
git status
```

---

### Chỉnh sửa Commit đã Push bằng `--amend`

Việc này làm thay đổi lịch sử Commit và có thể gây xung đột với đồng đội.

---

## Thực hành tốt

- Mỗi Commit chỉ nên giải quyết một vấn đề.
- Viết Commit Message rõ ràng.
- Kiểm tra `git diff --staged` trước khi Commit.
- Sử dụng `git add -p` để tạo các Commit nhỏ.
- Tránh Commit các tệp sinh ra từ quá trình build hoặc cấu hình cá nhân.

---

## Lệnh liên quan

- `git status`
- `git diff`
- `git restore`
- `git reset`

---

## Bài tiếp theo

→ [05. Branch & Switch](./05-branch-and-switch.md)
