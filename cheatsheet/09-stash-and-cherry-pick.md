## Stash & Cherry-pick

> Tạm lưu công việc đang thực hiện hoặc sao chép một Commit từ Branch khác mà không cần Merge toàn bộ Branch.

---

## Tổng quan

Trong quá trình phát triển phần mềm, bạn sẽ thường gặp các tình huống như:

- Đang làm dở một tính năng nhưng cần chuyển sang sửa lỗi gấp.
- Muốn đổi Branch nhưng chưa muốn Commit.
- Chỉ cần lấy một Commit từ Branch khác thay vì Merge toàn bộ Branch.

Git cung cấp hai công cụ rất hữu ích cho những tình huống này:

- **git stash**: Tạm lưu các thay đổi chưa Commit.
- **git cherry-pick**: Sao chép một hoặc nhiều Commit sang Branch hiện tại.

---

## Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git stash` | Lưu tạm các thay đổi |
| `git stash push -m` | Lưu tạm kèm mô tả |
| `git stash list` | Xem danh sách Stash |
| `git stash show` | Xem nội dung Stash |
| `git stash apply` | Khôi phục Stash |
| `git stash pop` | Khôi phục và xóa Stash |
| `git stash drop` | Xóa một Stash |
| `git stash clear` | Xóa toàn bộ Stash |
| `git cherry-pick` | Sao chép Commit |
| `git cherry-pick --continue` | Tiếp tục sau khi xử lý Conflict |
| `git cherry-pick --abort` | Hủy Cherry-pick |

---

## git stash

### Mục đích

Lưu tạm các thay đổi chưa Commit và đưa Working Tree về trạng thái sạch.

Đây là giải pháp phù hợp khi bạn cần chuyển sang công việc khác nhưng chưa muốn Commit.

### Cú pháp

```bash
git stash
```

Ví dụ

```bash
git stash
```

Sau khi thực hiện:

- Working Tree sạch.
- Các thay đổi được lưu trong Stash.
- Có thể chuyển Branch an toàn.

---

## Lưu Stash có mô tả

### Mục đích

Đặt tên để dễ nhận biết khi có nhiều Stash.

### Cú pháp

```bash
git stash push -m "Implement login page"
```

Ví dụ

```bash
git stash push -m "Fix payment bug"
```

---

## Xem danh sách Stash

### Mục đích

Hiển thị tất cả các Stash đã lưu.

### Cú pháp

```bash
git stash list
```

Ví dụ

```text
stash@{0}: On feature/login: Fix payment bug

stash@{1}: On develop: Update README
```

---

## Xem nội dung Stash

### Mục đích

Kiểm tra các thay đổi trong một Stash.

### Cú pháp

```bash
git stash show
```

Hiển thị chi tiết hơn:

```bash
git stash show -p
```

---

## Khôi phục Stash

### Mục đích

Khôi phục các thay đổi từ Stash.

Stash vẫn được giữ lại sau khi khôi phục.

### Cú pháp

```bash
git stash apply
```

Khôi phục một Stash cụ thể

```bash
git stash apply stash@{1}
```

---

## Khôi phục và xóa Stash

### Mục đích

Khôi phục thay đổi và xóa Stash ngay sau khi hoàn tất.

### Cú pháp

```bash
git stash pop
```

Đây là lệnh được sử dụng phổ biến nhất.

---

## Xóa Stash

### Xóa một Stash

```bash
git stash drop stash@{0}
```

---

### Xóa toàn bộ Stash

```bash
git stash clear
```

> **Cảnh báo:** Sau khi `clear`, các Stash sẽ bị xóa hoàn toàn.

---

## Khi nào nên sử dụng git stash?

- Đang làm dở nhưng cần chuyển Branch.
- Muốn Pull hoặc Rebase trước khi tiếp tục.
- Chưa đủ điều kiện để Commit.
- Muốn thử nghiệm nhanh một thay đổi.

Không nên dùng `git stash` như nơi lưu trữ lâu dài.

---

## git cherry-pick

### Mục đích

Sao chép một hoặc nhiều Commit từ Branch khác sang Branch hiện tại.

Không cần Merge toàn bộ Branch.

---

### Cú pháp

```bash
git cherry-pick <commit>
```

Ví dụ

```bash
git cherry-pick 8ab2f7d
```

Git sẽ tạo một Commit mới với nội dung giống Commit được chọn.

---

## Cherry-pick nhiều Commit

```bash
git cherry-pick commit1 commit2 commit3
```

Hoặc theo khoảng Commit

```bash
git cherry-pick commitA..commitB
```

---

## Cherry-pick khi xảy ra Conflict

Nếu xảy ra Conflict, Git sẽ tạm dừng quá trình Cherry-pick.

Các bước xử lý:

1. Sửa Conflict.
2. Đưa tệp vào Staging.

```bash
git add .
```

3. Tiếp tục.

```bash
git cherry-pick --continue
```

Nếu muốn hủy:

```bash
git cherry-pick --abort
```

---

## Khi nào nên sử dụng git cherry-pick?

Phù hợp khi:

- Chỉ cần lấy một bản vá (Hotfix).
- Chuyển một Commit sang Branch Release.
- Sao chép Commit giữa các Branch độc lập.
- Không muốn Merge toàn bộ Branch.

Không nên sử dụng để thay thế Merge trong quy trình phát triển thông thường.

---

## Sơ đồ lựa chọn

```text
Muốn chuyển thay đổi?

        │
        ▼

Chưa Commit?

        │
        ├──► git stash

        ▼

Đã Commit?

        │
        ├──► Chỉ cần 1 Commit?
        │          │
        │          └──► git cherry-pick
        │
        └──► Cần toàn bộ Branch?
                   │
                   ├──► git merge
                   │
                   └──► git rebase
```

---

## Các lỗi thường gặp

### Quên Stash đang tồn tại

Nhiều người tạo Stash nhưng quên khôi phục.

Kiểm tra định kỳ:

```bash
git stash list
```

---

### Lưu quá nhiều Stash

Quá nhiều Stash sẽ khó quản lý.

Nên:

- Đặt tên bằng `-m`.
- Xóa các Stash không còn sử dụng.

---

### Cherry-pick nhiều Commit liên tiếp

Việc Cherry-pick quá nhiều Commit có thể làm lịch sử Repository khó theo dõi.

Nếu cần chuyển cả một tính năng, hãy cân nhắc sử dụng Merge hoặc Rebase.

---

### Cherry-pick nhầm Commit

Luôn kiểm tra Commit trước khi Cherry-pick:

```bash
git show <commit>
```

---

## Thực hành tốt

- Chỉ sử dụng `git stash` cho các thay đổi ngắn hạn.
- Đặt mô tả cho Stash bằng `git stash push -m`.
- Dọn dẹp các Stash không còn sử dụng.
- Kiểm tra nội dung Commit trước khi Cherry-pick.
- Ưu tiên Merge hoặc Rebase nếu cần đồng bộ toàn bộ Branch.

---

## Lệnh liên quan

- `git merge`
- `git rebase`
- `git show`
- `git log`
- `git restore`

---

## Bài tiếp theo

→ [10. Tags & Release](./10-tags-and-release.md)