# Common Git Myths

> Phân biệt giữa những quan niệm phổ biến và cách Git thực sự hoạt động.

---

## Objective

Sau bài Lab này, bạn sẽ có thể:

- Nhận biết các hiểu lầm phổ biến về Git.
- Kiểm chứng chúng bằng các thí nghiệm nhỏ.
- Liên hệ với các khái niệm đã học trong Handbook.
- Hiểu vì sao Git có những hành vi tưởng như "kỳ lạ".

---

## Background

Trong quá trình sử dụng Git, nhiều khái niệm thường bị đơn giản hóa để dễ học.

Ví dụ:

- Branch là bản sao của project.
- Git lưu từng dòng thay đổi.
- `reset` xóa Commit.
- `push` gửi toàn bộ Repository.

Các cách giải thích này có thể hữu ích với người mới bắt đầu, nhưng không phản ánh đúng cách Git hoạt động.

Trong bài Lab này, chúng ta sẽ kiểm chứng từng quan niệm bằng các thí nghiệm thực tế.

---

## Myth 1 — Branch là bản sao của Repository

### Experiment

Khởi tạo Repository:

```bash
git init

echo "v1" > app.txt
git add .
git commit -m "Initial commit"
```

Tạo Branch mới:

```bash
git switch -c feature/demo
```

Liệt kê Object:

```bash
find .git/objects -type f
```

Hoặc:

```powershell
Get-ChildItem .git/objects -Recurse -File
```

---

### Observe

Sau khi tạo Branch:

- Không có Object mới.
- Không có Blob mới.
- Không có Tree mới.
- Không có Commit mới.

---

### Explain

Git chỉ tạo thêm một Reference:

```text
refs/heads/feature/demo
```

Branch không chứa dữ liệu.

Branch chỉ lưu Hash của Commit hiện tại.

---

### Plumbing Corner

Xem Hash của hai Branch:

```bash
git rev-parse main
git rev-parse feature/demo
```

Kết quả:

Hai Hash giống nhau.

Điều này chứng minh cả hai Branch đang cùng tham chiếu tới một Commit.

---

## Myth 2 — Git lưu từng thay đổi (Diff)

### Experiment

Thực hiện:

```bash
echo "Version 2" >> app.txt

git add .
git commit -m "Update"
```

Lấy Tree của HEAD:

```bash
git ls-tree HEAD
```

---

### Observe

Git hiển thị:

```text
100644 blob ...
```

Không có thông tin:

```text
+ Version 2
```

---

### Explain

Git lưu Snapshot.

Không lưu Diff như nhiều Version Control System truyền thống.

Diff được Git tính toán khi cần hiển thị.

---

### Plumbing Corner

Xem Commit:

```bash
git cat-file -p HEAD
```

Quan sát:

```text
tree ...
parent ...
author ...
```

Commit không chứa nội dung của tệp.

---

## Myth 3 — Reset xóa Commit

### Experiment

Tạo thêm Commit:

```bash
echo "Version 3" >> app.txt

git commit -am "Version 3"
```

Reset:

```bash
git reset --hard HEAD~1
```

Sau đó:

```bash
git reflog
```

---

### Observe

Commit vừa "mất" vẫn xuất hiện trong Reflog.

---

### Explain

Reset chỉ di chuyển Reference.

Commit vẫn còn trong Object Database.

---

### Plumbing Corner

Lấy Hash từ Reflog.

Sau đó:

```bash
git cat-file -p <commit-hash>
```

Bạn vẫn có thể đọc đầy đủ Commit đó.

---

## Myth 4 — HEAD là Commit

### Experiment

Thực hiện:

```bash
git symbolic-ref HEAD
```

---

### Observe

Ví dụ:

```text
refs/heads/main
```

---

### Explain

HEAD thường là một **symbolic reference**.

Nó trỏ tới:

```text
main

↓

Commit
```

Không trỏ trực tiếp tới Commit.

---

### Plumbing Corner

Kiểm tra:

```bash
cat .git/HEAD
```

Nội dung:

```text
ref: refs/heads/main
```

---

## Myth 5 — Push gửi toàn bộ Repository

### Experiment

Quan sát kích thước Repository:

```bash
git count-objects -v
```

Tạo Commit mới.

Push.

Tiếp tục:

```bash
git count-objects -v
```

---

### Observe

Chỉ có Object mới được tạo.

---

### Explain

Git truyền các Object mà Remote chưa có.

Không truyền lại toàn bộ Repository.

---

## Challenge

### Challenge 1

Checkout Tag:

```bash
git checkout v1.0.0
```

Sau đó:

```bash
git symbolic-ref HEAD
```

Điều gì xảy ra?

---

### Challenge 2

Thực hiện:

```bash
git rev-parse HEAD
```

Tiếp tục:

```bash
git rev-parse main
```

Hai kết quả có giống nhau không?

Giải thích.

---

### Challenge 3

Dùng:

```bash
git cat-file -t HEAD
```

Sau đó:

```bash
git cat-file -p HEAD
```

Hãy xác định:

- Đây là Object gì?
- Có Parent không?
- Tree ở đâu?

---

## Takeaway

Sau bài Lab này, bạn cần ghi nhớ:

- Branch không phải bản sao của Repository.
- Git lưu Snapshot, không lưu Diff.
- Commit không bị xóa ngay sau `reset`.
- HEAD thường là symbolic reference.
- Push chỉ truyền các Object còn thiếu.

---

## Related Handbook

- 02 — Git Object Database
- 03 — Working Tree, Index & HEAD
- 04 — Commit History & References
- 05 — Branch & Tag
- 06 — Remote & Tracking Branch
- 08 — History Rewrite
