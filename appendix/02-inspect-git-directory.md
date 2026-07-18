# Inspect `.git` Directory

> Khám phá trực tiếp thư mục `.git` để hiểu Git thực sự lưu trữ những gì.

---

## Objective

Sau bài Lab này, bạn sẽ có thể:

- Xác định vai trò của các thư mục trong `.git`.
- Quan sát Object Database.
- Đọc các Reference của Branch và HEAD.
- Hiểu Git lưu Repository ở đâu.
- Liên hệ giữa cấu trúc `.git` và các chương trong Handbook.

---

## Background

Trong Handbook, chúng ta đã biết:

```text
Repository

├── Working Tree
├── Index
└── Object Database
```

Toàn bộ lịch sử Git thực chất nằm trong:

```text
.git/
```

Thư mục Working Tree chỉ là phần bạn nhìn thấy.

Mọi thông tin về Commit, Branch, Tag và lịch sử đều được lưu trong `.git`.

Hôm nay chúng ta sẽ mở nó ra và quan sát.

---

## Setup

Tạo Repository mới:

```bash
mkdir git-lab
cd git-lab

git init

echo "## Git Lab" > README.md

git add .
git commit -m "Initial commit"
```

Sau đó hiển thị:

```bash
ls -la .git
```

(Windows PowerShell)

```powershell
Get-ChildItem .git -Force
```

---

## Experiment 1 — Quan sát cấu trúc `.git`

Mở thư mục:

```text
.git/
```

Bạn sẽ thấy tương tự:

```text
.git/

├── HEAD
├── config
├── description
├── hooks/
├── index
├── info/
├── logs/
├── objects/
├── refs/
└── COMMIT_EDITMSG
```

---

## Observe

Tự trả lời:

- File nào chứa HEAD?
- Object nằm ở đâu?
- Branch nằm ở đâu?
- Git Config nằm ở đâu?

---

## Explain

| Thành phần | Vai trò |
|------------|----------|
| HEAD | Reference hiện tại |
| objects | Object Database |
| refs | Branch và Tag |
| logs | Reflog |
| index | Staging Area |
| config | Cấu hình Repository |
| hooks | Git Hooks |

Đây chính là các thành phần đã được giới thiệu xuyên suốt Handbook.

---

## Experiment 2 — Đọc HEAD

Hiển thị:

Linux / macOS

```bash
cat .git/HEAD
```

Windows PowerShell

```powershell
Get-Content .git/HEAD
```

---

## Observe

Ví dụ:

```text
ref: refs/heads/main
```

---

## Explain

HEAD không chứa Commit.

HEAD chỉ chứa:

```text
Reference
```

Điều này xác nhận mô hình:

```text
HEAD
 │
 ▼
main
 │
 ▼
Commit
```

---

## Experiment 3 — Quan sát Branch

Liệt kê:

```bash
ls .git/refs/heads
```

Windows:

```powershell
Get-ChildItem .git/refs/heads
```

Ví dụ:

```text
main
```

Hiển thị:

```bash
cat .git/refs/heads/main
```

---

## Observe

Bạn sẽ thấy:

```text
1d4f7a8...
```

---

## Explain

Đây chính là Hash của Commit mới nhất.

Branch thực chất chỉ là:

```text
Tên

↓

Hash Commit
```

Không có mã nguồn nào được lưu trong Branch.

---

## Experiment 4 — Quan sát Object Database

Liệt kê:

```bash
find .git/objects -type f
```

Windows PowerShell

```powershell
Get-ChildItem .git/objects -Recurse -File
```

Bạn sẽ thấy:

```text
.git/objects/

aa/

b4f...

2c/

91d...
```

---

## Observe

Các Object được chia thành nhiều thư mục nhỏ.

Không có tên:

```text
README.md
```

hay

```text
main.py
```

---

## Explain

Git lưu Object theo Hash.

Ví dụ:

Hash

```text
aa3d2bc9...
```

được lưu:

```text
objects/

aa/

3d2bc9...
```

Hai ký tự đầu tạo thư mục.

Phần còn lại là tên tệp.

Nhờ vậy Git có thể lưu hàng triệu Object mà vẫn hoạt động hiệu quả.

---

## Experiment 5 — Quan sát Index

Hiển thị:

```bash
ls -lh .git/index
```

Windows:

```powershell
Get-Item .git/index
```

Tiếp tục:

```bash
echo "Hello" >> README.md
```

Chưa Add.

Quan sát:

```bash
git status
```

Sau đó:

```bash
git add README.md
```

Quan sát lại kích thước:

```bash
ls -lh .git/index
```

---

## Observe

File `index` đã thay đổi.

---

## Explain

Index chính là Staging Area.

Nó không phải một khái niệm trừu tượng.

Nó là một file thật:

```text
.git/index
```

Git cập nhật file này sau mỗi lần:

```bash
git add
```

---

## Experiment 6 — Quan sát Reflog

Thực hiện:

```bash
git commit --allow-empty -m "Second"

git reset --hard HEAD~1
```

Sau đó:

```bash
git reflog
```

---

## Observe

Bạn vẫn thấy Commit vừa Reset.

---

## Explain

Reflog nằm trong:

```text
.git/logs/
```

Nó ghi lại:

- HEAD từng ở đâu.
- Branch từng di chuyển thế nào.

Đây là lý do Git có thể khôi phục nhiều tình huống tưởng như đã mất dữ liệu.

---

## Challenge

### Challenge 1

Tạo Branch:

```bash
git switch -c feature/demo
```

Quan sát:

```text
.git/refs/heads/
```

Điều gì thay đổi?

---

### Challenge 2

Tạo Tag:

```bash
git tag v1.0.0
```

Quan sát:

```text
.git/refs/tags/
```

Có file mới không?

Nội dung là gì?

---

### Challenge 3

Sau mỗi Commit:

Quan sát:

```text
.git/objects/
```

Có bao nhiêu Object mới?

Thử giải thích:

- Blob
- Tree
- Commit

Object nào vừa được tạo?

---

### Challenge 4

Mở:

```text
.git/config
```

Bạn có nhận ra:

- user
- remote
- branch
- merge

không?

Liên hệ với chương 06 của Handbook.

---

## Takeaway

Sau bài Lab này, bạn cần ghi nhớ:

- `.git` là trái tim của Repository.
- `HEAD` chỉ là một Reference.
- Branch được lưu trong `refs/heads/`.
- Tag được lưu trong `refs/tags/`.
- Object Database nằm trong `objects/`.
- Staging Area chính là file `index`.
- Reflog nằm trong `logs/`.
- Hầu hết những gì Git làm đều có thể quan sát trực tiếp trong `.git`.

---

## Liên hệ Handbook

Sau khi hoàn thành bài Lab này, hãy thử đối chiếu lại:

- Chương 02 — Git Object Database
- Chương 03 — Working Tree, Index & HEAD
- Chương 04 — Commit History & References
- Chương 05 — Branch & Tag
- Chương 06 — Remote & Tracking Branch

Bạn sẽ thấy mọi khái niệm trong Handbook đều có biểu diễn cụ thể trong thư mục `.git`.
