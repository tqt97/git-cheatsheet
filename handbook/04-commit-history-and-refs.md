# Commit History & References

> Hiểu cách Git quản lý lịch sử bằng Commit, Reference và HEAD.

---

## Tổng quan

Một trong những hiểu lầm phổ biến nhất là:

> Git lưu lịch sử dưới dạng danh sách các Commit.

Thực tế, Git lưu lịch sử dưới dạng **đồ thị có hướng không chu trình (Directed Acyclic Graph - DAG)**.

Branch, Tag và HEAD không chứa mã nguồn.

Chúng chỉ là **Reference** trỏ tới các Commit trong đồ thị này.

Nếu hiểu chương này, bạn sẽ biết:

- Vì sao Branch gần như được tạo tức thời.
- Vì sao Merge chỉ là kết nối các Commit.
- Vì sao Rebase làm thay đổi lịch sử.
- Vì sao HEAD luôn "di chuyển".

---

## Mental Model

Hãy tưởng tượng Commit giống như các ga tàu.

```text
A ---- B ---- C ---- D
```

Mỗi ga biết:

- Nó là ai.
- Nó đến từ ga nào.

Git chỉ cần biết Commit hiện tại, sau đó có thể lần ngược toàn bộ lịch sử.

---

## Git Internals Diagram

```text
                HEAD
                 │
                 ▼
              main
                 │
                 ▼
+---------+    +---------+    +---------+    +---------+
|Commit A | -> |Commit B | -> |Commit C | -> |Commit D |
+---------+    +---------+    +---------+    +---------+
```

Branch chỉ là một Reference.

HEAD chỉ trỏ tới Branch hiện tại.

---

## Commit là gì?

Commit không chứa toàn bộ Repository.

Một Commit gồm các thông tin chính:

```text
Commit

├── Tree
├── Parent
├── Author
├── Committer
├── Timestamp
└── Message
```

Ví dụ:

```text
Commit

↓

Tree

↓

Blob
```

Tree mô tả Snapshot của Repository.

---

## Parent Commit

Mỗi Commit (trừ Commit đầu tiên) đều biết Commit cha của mình.

Ví dụ:

```text
A

↓

B

↓

C

↓

D
```

Commit D lưu:

```text
Parent = C
```

Commit C lưu:

```text
Parent = B
```

Git chỉ cần đi theo các Parent để dựng lại toàn bộ lịch sử.

---

## Commit History thực chất là gì?

Điều Git lưu không phải:

```text
Danh sách Commit
```

mà là:

```text
Commit

↓

Parent

↓

Parent

↓

Parent
```

Đây chính là DAG.

---

## Reference là gì?

Reference (Ref) là một tên dễ nhớ trỏ tới một Commit.

Ví dụ:

```text
main

↓

Commit D
```

Hoặc:

```text
feature/login

↓

Commit F
```

Ref giúp chúng ta không phải nhớ Hash dài của Commit.

---

## Branch thực chất là gì?

Nhiều người nghĩ:

> Branch là một bản sao của Repository.

Điều này không đúng.

Branch chỉ là:

```text
feature/login

↓

Commit F
```

Khi Commit mới:

```text
Commit G
```

Git chỉ cập nhật:

```text
feature/login

↓

Commit G
```

Không có dữ liệu nào bị sao chép.

Đó là lý do tạo Branch gần như tức thời.

---

## HEAD là gì?

HEAD là Reference đặc biệt.

Thông thường:

```text
HEAD

↓

main

↓

Commit D
```

Khi chuyển Branch:

```bash
git switch develop
```

HEAD đổi thành:

```text
HEAD

↓

develop

↓

Commit X
```

---

## Detached HEAD

Nếu Checkout trực tiếp một Commit:

```bash
git checkout a13b7d2
```

Sẽ có:

```text
HEAD

↓

Commit B
```

HEAD không còn trỏ tới Branch.

Nếu Commit tiếp:

```text
Commit B

↓

Commit C
```

Commit C sẽ "mồ côi" nếu không tạo Branch mới.

---

## Inside Git

Giả sử:

```bash
git commit -m "Add login"
```

Git sẽ:

1. Tạo Blob mới (nếu có thay đổi).
2. Tạo Tree mới.
3. Tạo Commit mới.
4. Ghi Parent là Commit hiện tại.
5. Cập nhật Branch.
6. HEAD tự động trỏ tới Commit mới.

Thực chất:

```text
Commit D

↓

Commit E

↓

main
```

Branch chỉ được cập nhật sang Commit mới.

---

## Ví dụ thực tế

Ban đầu:

```text
main

↓

A
```

Commit:

```bash
git commit
```

Sau đó:

```text
main

↓

B
```

Git không sửa Commit A.

Git tạo Commit B rồi cập nhật `main`.

Commit luôn là **bất biến (Immutable)**.

---

## Vì sao Commit không thể sửa?

Hash của Commit được tính từ:

- Tree
- Parent
- Author
- Message
- Timestamp

Nếu sửa một thông tin nhỏ:

Hash sẽ thay đổi.

Do đó:

```text
A

↓

B

↓

C
```

sẽ trở thành:

```text
A

↓

B'

↓

C'
```

Đây là lý do Rebase và Amend tạo Commit mới thay vì sửa Commit cũ.

---

## Common Misconceptions

### Branch chứa dữ liệu

❌ Sai.

Branch chỉ là Reference.

---

### Commit có thể chỉnh sửa

❌ Sai.

Commit là Immutable.

Git chỉ tạo Commit mới.

---

### HEAD là Commit

❌ Chưa chính xác.

Thông thường:

HEAD → Branch → Commit

Chỉ trong Detached HEAD, HEAD mới trỏ trực tiếp tới Commit.

---

### Git lưu lịch sử dạng danh sách

❌ Sai.

Git lưu DAG.

---

## 🔬 Experiment 1 - Quan sát Branch di chuyển

#### Dự đoán trước khi chạy

Sau khi tạo Commit mới:

- Commit cũ có thay đổi không?
- `main` sẽ trỏ tới đâu?

#### Thực hành

```bash
git init

echo "v1" > app.txt
git add .
git commit -m "v1"

git log --oneline

echo "v2" >> app.txt
git add .
git commit -m "v2"

git log --oneline --graph
```

#### Quan sát

- Có hai Commit khác nhau.
- Commit đầu tiên vẫn giữ nguyên.
- `main` được cập nhật sang Commit mới.

---

## 🔬 Experiment 2 - Quan sát HEAD

#### Dự đoán

HEAD sẽ thay đổi thế nào khi chuyển Branch?

#### Thực hành

```bash
git switch -c feature/login

git branch

cat .git/HEAD
```

#### Quan sát

Bạn sẽ thấy:

```text
ref: refs/heads/feature/login
```

HEAD không chứa mã nguồn.

HEAD chỉ chứa Reference.

---

## 🔬 Experiment 3 - Detached HEAD

#### Dự đoán

Điều gì xảy ra nếu Checkout trực tiếp một Commit?

#### Thực hành

```bash
git log --oneline

git checkout <commit-hash>

git status
```

#### Quan sát

Git hiển thị thông báo:

```text
You are in 'detached HEAD' state.
```

Lúc này HEAD không còn trỏ tới Branch.

---

## Tóm tắt

Sau chương này cần ghi nhớ:

- Commit là Immutable.
- Mỗi Commit biết Commit cha của mình.
- Git lưu lịch sử bằng DAG.
- Branch chỉ là một Reference.
- HEAD thường trỏ tới Branch.
- Tạo Branch không sao chép dữ liệu.
- Commit mới không sửa Commit cũ mà tạo Object mới.
- Hiểu Commit và Reference sẽ giúp bạn dễ dàng nắm được Merge, Rebase và Reset.

---

## Chương tiếp theo

→ [05. Branch & Tag](./05-branch-and-tag.md)
