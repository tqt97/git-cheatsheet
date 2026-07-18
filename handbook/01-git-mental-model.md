# Git Mental Model

> Trước khi học Git, hãy hiểu Git đang quản lý điều gì.

---

## Tổng quan

Nhiều người mới học Git thường nghĩ:

> Git lưu các tệp.

Thực tế:

> **Git lưu lịch sử của dự án.**

Đây là khác biệt quan trọng nhất.

Git không quan tâm bạn có bao nhiêu tệp.

Git quan tâm:

- Nội dung thay đổi.
- Thời điểm thay đổi.
- Ai thay đổi.
- Thay đổi dựa trên lịch sử nào.

---

## Git không phải hệ thống lưu file

Giả sử:

```text
project/

README.md

main.py

config.json
```

Sau khi Commit,

Git không chỉ lưu ba tệp này.

Git còn lưu:

- Snapshot của toàn bộ Repository.
- Metadata.
- Author.
- Commit Message.
- Parent Commit.

---

## Git quản lý Snapshot

Nhiều Version Control System cũ hoạt động như sau:

```text
Version 1

↓

Lưu thay đổi

↓

Version 2

↓

Lưu thay đổi

↓

Version 3
```

Git khác.

Git xem mỗi Commit là một Snapshot.

```text
Commit A

↓

Snapshot

Commit B

↓

Snapshot

Commit C

↓

Snapshot
```

Nếu một tệp không thay đổi, Git sẽ không lưu lại nội dung của tệp đó.

Thay vào đó, Git tham chiếu đến dữ liệu đã tồn tại.

---

## Repository là gì?

Repository không chỉ là thư mục chứa mã nguồn.

Repository gồm:

```text
Repository

│

├── Working Tree

├── Index

└── Object Database
```

Ba thành phần này phối hợp với nhau trong hầu hết mọi lệnh Git.

---

## Ba khu vực làm việc

```text
Editor

↓

Working Tree

↓

git add

↓

Index

↓

git commit

↓

Object Database
```

Đây là luồng dữ liệu quan trọng nhất trong Git.

Nếu hiểu sơ đồ này, bạn sẽ hiểu phần lớn các lệnh Git.

---

## Commit không lưu sự khác biệt

Một hiểu lầm phổ biến:

> Git lưu từng dòng thay đổi.

Thực tế:

Git lưu Snapshot của toàn bộ Repository.

Ví dụ

Commit A

```text
README

main.py
```

Commit B

```text
README

main.py

config.json
```

Commit B đại diện cho toàn bộ trạng thái của Repository tại thời điểm đó.

---

## Git là đồ thị

Lịch sử Git không phải danh sách.

Đó là Directed Acyclic Graph (DAG).

```text
A

↓

B

↓

C

├── D

│

└── E
```

Điều này cho phép:

- Branch.
- Merge.
- Rebase.
- Cherry-pick.

---

## Mọi thứ đều là tham chiếu

Trong Git:

- Branch là Reference.
- Tag là Reference.
- HEAD là Reference.

Không có Branch nào chứa mã nguồn.

Mã nguồn nằm trong Object Database.

Branch chỉ trỏ tới Commit mới nhất.

---

## Git không xóa dữ liệu ngay

Ngay cả khi:

```bash
git reset --hard

git branch -D

git rebase
```

Commit thường vẫn còn trong Object Database.

Đó là lý do `git reflog` có thể khôi phục dữ liệu trong nhiều trường hợp.

---

## Mô hình tư duy cần ghi nhớ

```text
Code

↓

Working Tree

↓

Index

↓

Commit

↓

Branch

↓

Remote
```

Đây là chuỗi quan trọng nhất trong toàn bộ Git.

---

## Những hiểu lầm phổ biến

❌ Git lưu từng file.

✔ Git lưu Snapshot.

---

❌ Branch là bản sao của project.

✔ Branch chỉ là Reference.

---

❌ Merge là sao chép code.

✔ Merge là kết nối lịch sử Commit.

---

❌ Rebase là Merge.

✔ Rebase tạo lại Commit trên một nền lịch sử mới.

---

## Tóm tắt

Sau chương này, bạn cần ghi nhớ:

- Git quản lý lịch sử, không chỉ quản lý tệp.
- Commit là Snapshot của Repository.
- Git được xây dựng trên Object Database.
- Branch và Tag chỉ là Reference.
- Lịch sử Git là một DAG.
- Hầu hết các lệnh Git đều là thao tác trên các Reference hoặc Object.

---

## Chương tiếp theo

→ [02. Git Object Database](./02-git-object-database.md)