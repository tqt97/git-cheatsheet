# History Rewrite

> Hiểu cách Git "viết lại lịch sử" mà không thực sự sửa các Commit đã tồn tại.

---

## Tổng quan

Trong các chương trước, chúng ta đã biết:

- Commit là Immutable.
- Branch chỉ là Reference.
- HEAD trỏ tới Branch hiện tại.
- Rebase tạo Commit mới.

Vậy câu hỏi đặt ra là:

> Nếu Commit không thể sửa, tại sao Git vẫn có `commit --amend`, `reset` hay `rebase -i`?

Câu trả lời là:

Git **không chỉnh sửa Commit cũ**.

Git tạo ra một lịch sử mới rồi cập nhật Reference sang lịch sử đó.

Đây chính là bản chất của History Rewrite.

---

## Mental Model

Hãy tưởng tượng lịch sử Git giống như một tuyến đường.

Ban đầu:

```text
A → B → C → D
```

Bạn muốn thay Commit `C`.

Git không thể sửa `C`.

Git tạo:

```text
A → B → C' → D'
```

Sau đó cập nhật Branch:

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C' → D'
```

Lịch sử cũ vẫn tồn tại trong Object Database cho đến khi được dọn dẹp.

---

## Git Internals Diagram

### Trước Rewrite

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C → D
```

### Sau Rewrite

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C' → D'
```

Commit:

```text
C
D
```

không bị sửa.

Git chỉ tạo:

```text
C'
D'
```

và cập nhật `main`.

---

## Immutable Commit

Commit được định danh bằng Hash.

Hash phụ thuộc vào:

- Tree
- Parent
- Author
- Committer
- Timestamp
- Message

Ví dụ:

```text
Commit C

Hash = a13b7d...
```

Nếu đổi Message:

```text
Fix login

↓

Fix login bug
```

Hash thay đổi.

Kết quả:

```text
Commit C'

Hash = 7fd912...
```

Đây là Commit hoàn toàn mới.

---

## `git commit --amend`

### Điều gì thực sự xảy ra?

Nhiều người nghĩ:

```bash
git commit --amend
```

sửa Commit cuối.

Thực tế:

```text
A → B → C
```

↓

```text
A → B → C'
```

Git tạo Commit mới rồi cập nhật Branch.

Commit C cũ vẫn còn trong Object Database.

---

## `git reset`

Reset thường bị hiểu nhầm là "xóa Commit".

Thực chất:

Reset chủ yếu là thao tác trên Reference.

Ví dụ:

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C → D
```

Thực hiện:

```bash
git reset --hard B
```

Kết quả:

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B
```

Commit:

```text
C
D
```

vẫn tồn tại trong Object Database.

Chỉ còn không được Branch nào tham chiếu.

---

## Soft, Mixed và Hard Reset

| Chế độ | HEAD | Index | Working Tree |
|--------|------|-------|--------------|
| `--soft` | ✔ | ✘ | ✘ |
| `--mixed` | ✔ | ✔ | ✘ |
| `--hard` | ✔ | ✔ | ✔ |

Ý nghĩa:

- ✔ = thay đổi
- ✘ = giữ nguyên

---

## Interactive Rebase

Interactive Rebase cho phép:

- Đổi thứ tự Commit.
- Gộp Commit.
- Chỉnh sửa Commit.
- Xóa Commit.
- Chia nhỏ Commit.

Ví dụ:

```text
A → B → C → D
```

Sau:

```bash
git rebase -i B
```

Có thể trở thành:

```text
A → B → C'
```

Hoặc:

```text
A → B → D' → C'
```

Hoặc:

```text
A → B → CD'
```

Tất cả đều là lịch sử mới.

---

## Rebase không sửa Commit

Trong Interactive Rebase:

```text
C
```

không bị chỉnh sửa.

Git tạo:

```text
C'
```

Điều này đúng với mọi thao tác:

- squash
- reword
- edit
- drop

---

## Reflog

Một câu hỏi quan trọng:

Nếu Reset hoặc Rebase làm mất Branch thì Git tìm lại bằng cách nào?

Câu trả lời:

```text
reflog
```

Reflog ghi lại:

- HEAD từng ở đâu.
- Branch từng trỏ tới Commit nào.

Ví dụ:

```bash
git reflog
```

```text
HEAD@{0}

HEAD@{1}

HEAD@{2}
```

Đây là "nhật ký di chuyển" của các Reference.

---

## Inside Git

Giả sử:

```bash
git rebase main
```

Git thực hiện:

1. Tìm Merge Base.
2. Xác định các Commit cần phát lại.
3. Tạo Commit mới.
4. Cập nhật Branch.
5. Ghi thay đổi vào Reflog.

Không có Commit nào bị sửa.

---

## Garbage Collection

Nếu:

```text
C
D
```

không còn Branch hoặc Tag nào tham chiếu.

Chúng trở thành:

```text
Dangling Commit
```

Git **không xóa ngay**.

Sau một khoảng thời gian, khi chạy cơ chế dọn dẹp (garbage collection), các Object không còn được tham chiếu mới có thể bị loại bỏ.

Đó là lý do `git reflog` thường có thể cứu dữ liệu vừa "mất".

---

## Ví dụ thực tế

Ban đầu:

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C → D
```

Sau:

```bash
git commit --amend
```

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C → D'
```

Commit D cũ vẫn tồn tại.

Branch chỉ chuyển sang D'.

---

## Common Misconceptions

### Reset xóa Commit

❌ Sai.

Reset chủ yếu di chuyển Reference.

---

### Amend sửa Commit

❌ Sai.

Amend tạo Commit mới.

---

### Rebase sửa lịch sử

❌ Sai.

Rebase tạo lịch sử mới.

---

### Commit bị mất là không thể khôi phục

❌ Sai.

Nếu Object vẫn còn và Reflog chưa hết hạn, thường có thể khôi phục.

---

## 🔬 Experiment 1 - Quan sát Commit --amend

#### Dự đoán

Hash Commit có thay đổi không?

#### Thực hành

```bash
echo "v1" > app.txt
git add .
git commit -m "v1"

git log --oneline

git commit --amend -m "version 1"

git log --oneline
```

#### Quan sát

Hash của Commit thay đổi.

#### 💡 Giải thích

Git tạo Commit mới với Message mới.

Commit cũ không bị sửa.

---

## 🔬 Experiment 2 - Quan sát Reset

#### Dự đoán

Sau `reset --hard`, Commit cũ còn tồn tại không?

#### Thực hành

```bash
git reset --hard HEAD~1

git reflog
```

#### Quan sát

Commit vừa "mất" vẫn xuất hiện trong Reflog.

#### 💡 Giải thích

Branch đã di chuyển, nhưng Object vẫn còn trong Object Database.

---

## 🔬 Experiment 3 - Khôi phục bằng Reflog

#### Dự đoán

Có thể quay lại Commit vừa Reset không?

#### Thực hành

```bash
git reflog

git reset --hard HEAD@{1}
```

#### Quan sát

Repository trở về trạng thái trước khi Reset.

#### 💡 Giải thích

Reflog lưu lịch sử di chuyển của HEAD và các Reference, cho phép Git tìm lại Commit đã không còn được Branch tham chiếu.

---

## ⚠️ Pitfall

### Rewrite lịch sử sau khi đã Push

Ví dụ:

```bash
git push origin main
```

Sau đó:

```bash
git rebase -i

git push --force-with-lease
```

Lúc này:

- Hash Commit thay đổi.
- Lịch sử trên Remote bị viết lại.
- Đồng nghiệp có thể gặp xung đột khi Pull hoặc Push.

**Khuyến nghị:**

- Chỉ Rewrite lịch sử trên Branch cá nhân hoặc trước khi chia sẻ.
- Với Branch dùng chung, ưu tiên tạo Commit mới (`git revert`) thay vì Rewrite.

---

## Tóm tắt

Sau chương này cần ghi nhớ:

- Commit là Immutable.
- Git không sửa Commit.
- History Rewrite luôn tạo Commit mới hoặc di chuyển Reference.
- `commit --amend` tạo Commit mới.
- `rebase -i` tạo lịch sử mới.
- `reset` chủ yếu cập nhật Reference.
- `reflog` ghi lại lịch sử di chuyển của HEAD và các Reference.
- Các Commit không còn được tham chiếu chưa bị xóa ngay và thường vẫn có thể khôi phục trong một khoảng thời gian.
