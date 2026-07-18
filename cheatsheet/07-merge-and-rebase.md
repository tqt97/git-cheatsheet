# Merge & Rebase

> Kết hợp thay đổi từ nhiều Branch và quản lý lịch sử Commit.

---

## Tổng quan

Trong quá trình phát triển phần mềm, nhiều Branch sẽ được tạo ra để phát triển tính năng, sửa lỗi hoặc thử nghiệm.

Để đưa các thay đổi này trở lại Branch chính, Git cung cấp hai cơ chế phổ biến:

- **Merge**: Kết hợp lịch sử của hai Branch.
- **Rebase**: Di chuyển các Commit sang một nền lịch sử mới.

Cả hai đều giúp đồng bộ mã nguồn, nhưng tạo ra lịch sử Commit khác nhau và phù hợp với những tình huống khác nhau.

---

## Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git merge` | Gộp một Branch vào Branch hiện tại |
| `git merge --no-ff` | Luôn tạo Merge Commit |
| `git rebase` | Di chuyển Commit sang nền mới |
| `git rebase -i` | Rebase tương tác để chỉnh sửa lịch sử |
| `git cherry-pick` | Sao chép một hoặc nhiều Commit sang Branch hiện tại |

---

## git merge

### Mục đích

Kết hợp các thay đổi từ một Branch khác vào Branch hiện tại.

Git sẽ giữ nguyên lịch sử của cả hai Branch.

### Cú pháp

```bash
git merge <branch>
```

Ví dụ

```bash
git switch main

git merge feature/login
```

Sau khi Merge, các Commit từ `feature/login` sẽ xuất hiện trên `main`.

---

## Fast-forward Merge

Nếu Branch hiện tại chưa có Commit mới kể từ khi tạo Branch con, Git sẽ thực hiện **Fast-forward Merge**.

Ví dụ

```text
A --- B --- C (main)
            \
             D --- E (feature)
```

Sau Merge

```text
A --- B --- C --- D --- E (main)
```

Không tạo Merge Commit mới.

---

## Merge Commit

Nếu cả hai Branch đều có thêm Commit mới, Git sẽ tạo Merge Commit.

Ví dụ

```text
A --- B --- C -------- M (main)
            \         /
             D --- E
```

Trong đó:

```
M
```

là Merge Commit.

---

## Luôn tạo Merge Commit

### Mục đích

Buộc Git tạo Merge Commit ngay cả khi có thể Fast-forward.

### Cú pháp

```bash
git merge --no-ff feature/login
```

Thường được sử dụng khi muốn giữ rõ lịch sử của từng Feature Branch.

---

## Merge Conflict

Conflict xảy ra khi hai Branch cùng chỉnh sửa một vị trí trong tệp.

Ví dụ

```text
<<<<<<< HEAD

Code trên main

=======

Code trên feature

>>>>>>> feature/login
```

Các bước xử lý:

1. Chỉnh sửa nội dung bị Conflict.
2. Lưu tệp.
3. Đưa tệp vào Staging.

```bash
git add .
```

4. Hoàn tất Merge.

```bash
git commit
```

---

## git rebase

### Mục đích

Di chuyển các Commit của Branch hiện tại lên Commit mới nhất của Branch khác.

Khác với Merge, Rebase giúp lịch sử Commit trở nên tuyến tính hơn.

### Cú pháp

```bash
git rebase main
```

Ví dụ

```bash
git switch feature/login

git rebase main
```

---

## Rebase hoạt động như thế nào?

Ban đầu

```text
A --- B --- C (main)
      \
       D --- E (feature)
```

Sau khi `main` có thêm Commit

```text
A --- B --- C --- F (main)
      \
       D --- E
```

Sau khi Rebase

```text
A --- B --- C --- F (main)
                \
                 D' --- E'
```

Git tạo lại các Commit `D` và `E` trên nền Commit mới.

Do đó Hash của Commit sẽ thay đổi.

---

## Interactive Rebase

### Mục đích

Cho phép chỉnh sửa lịch sử Commit.

Có thể:

- Đổi Commit Message.
- Gộp nhiều Commit.
- Xóa Commit.
- Thay đổi thứ tự Commit.

### Cú pháp

```bash
git rebase -i HEAD~3
```

Ví dụ

```text
pick a123456 Add login

pick b234567 Fix typo

pick c345678 Update README
```

Có thể đổi thành

```text
pick a123456 Add login

squash b234567 Fix typo

pick c345678 Update README
```

Git sẽ gộp hai Commit đầu thành một.

---

## git cherry-pick

### Mục đích

Sao chép một Commit từ Branch khác sang Branch hiện tại.

Không cần Merge toàn bộ Branch.

### Cú pháp

```bash
git cherry-pick <commit>
```

Ví dụ

```bash
git cherry-pick 8ab2f7d
```

Thường dùng khi:

- Chỉ cần một bản vá.
- Hotfix.
- Sao chép Commit giữa các Branch Release.

---

## Merge hay Rebase?

| Merge | Rebase |
|--------|---------|
| Giữ nguyên lịch sử | Viết lại lịch sử |
| Có Merge Commit | Không tạo Merge Commit |
| An toàn cho Branch đã chia sẻ | Chỉ nên dùng với Branch cá nhân |
| Dễ theo dõi quá trình phát triển | Lịch sử gọn gàng hơn |

---

## Khi nào nên dùng Merge?

- Hoàn thành Feature Branch.
- Làm việc trên Branch đã được chia sẻ.
- Muốn giữ nguyên lịch sử phát triển.

---

## Khi nào nên dùng Rebase?

- Đồng bộ Feature Branch với `main`.
- Dọn dẹp lịch sử Commit trước khi Merge.
- Làm việc trên Branch cá nhân.

---

## Các lỗi thường gặp

### Rebase Branch đã Push

Việc Rebase sẽ thay đổi Hash của Commit.

Nếu Branch đã được nhiều người sử dụng, Rebase có thể gây xung đột khi đồng bộ.

---

### Force Push sau Rebase

Sau Rebase thường phải Push lại.

Không nên sử dụng:

```bash
git push --force
```

Nên sử dụng:

```bash
git push --force-with-lease
```

---

### Merge khi Working Tree chưa sạch

Nếu còn thay đổi chưa Commit hoặc chưa Staging, Merge hoặc Rebase có thể thất bại.

Kiểm tra trước bằng:

```bash
git status
```

---

### Cherry-pick nhiều Commit liên tiếp

Cherry-pick quá nhiều Commit có thể làm lịch sử trở nên khó hiểu.

Nếu cần chuyển toàn bộ tính năng, nên Merge hoặc Rebase.

---

## Thực hành tốt

- Luôn `git fetch` trước khi Merge hoặc Rebase.
- Chỉ Rebase trên Branch cá nhân.
- Không Rebase lịch sử đã chia sẻ nếu chưa thống nhất với nhóm.
- Sử dụng `git merge --no-ff` nếu muốn giữ rõ lịch sử của từng Feature Branch.
- Kiểm tra kỹ Conflict trước khi hoàn tất Merge hoặc Rebase.
- Ưu tiên `git cherry-pick` khi chỉ cần một hoặc vài Commit.

---

## Lệnh liên quan

- `git fetch`
- `git pull`
- `git push`
- `git branch`
- `git switch`

---

## Bài tiếp theo

→ [08. Undo & Recovery](./08-undo-and-recovery.md)