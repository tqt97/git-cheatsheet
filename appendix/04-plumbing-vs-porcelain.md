# Plumbing vs Porcelain

> Hiểu mối quan hệ giữa các lệnh Git hằng ngày (Porcelain) và các lệnh nội bộ (Plumbing).

---

## Objective

Sau bài Lab này, bạn sẽ có thể:

- Phân biệt Porcelain và Plumbing.
- Hiểu một lệnh Git cấp cao được xây dựng từ những thao tác cấp thấp nào.
- Sử dụng một số Plumbing Commands để quan sát Repository.
- Có nền tảng để đọc tài liệu Git Internals hoặc mã nguồn Git.

---

## Background

Trong suốt Cheat Sheet và Handbook, chúng ta chủ yếu sử dụng các lệnh như:

```bash
git add
git commit
git switch
git log
git status
```

Đây đều là **Porcelain Commands**.

Porcelain được thiết kế để:

- dễ nhớ
- ổn định
- phục vụ người dùng cuối

Bên dưới chúng là các **Plumbing Commands**.

Plumbing không hướng đến người dùng thông thường mà cung cấp các thao tác ở mức thấp để:

- tạo Object
- đọc Object
- cập nhật Reference
- xây dựng Tree
- duyệt Commit Graph

Nhiều lệnh Porcelain thực chất chỉ là sự kết hợp của nhiều Plumbing Commands.

---

## Porcelain vs Plumbing

| Porcelain | Plumbing |
|-----------|----------|
| Dễ sử dụng | Mức thấp |
| Hướng tới người dùng | Hướng tới cơ chế |
| Tự động xử lý nhiều bước | Mỗi lệnh thực hiện một việc nhỏ |
| Giao diện ổn định | Chủ yếu phục vụ nội bộ và công cụ |

Ví dụ:

```bash
git commit
```

không tạo Commit bằng một thao tác duy nhất.

Nó thực hiện nhiều bước liên tiếp.

---

## Lab 1 — Commit được tạo như thế nào?

### Setup

```bash
git init

echo "Hello Git" > app.txt

git add app.txt
```

---

### Experiment

Quan sát trạng thái:

```bash
git status
```

Sau đó:

```bash
git write-tree
```

---

### Observe

Lệnh trả về một Hash.

Ví dụ:

```text
4b825dc642...
```

---

### Explain

`git write-tree` tạo một **Tree Object** từ nội dung hiện có trong Index.

Chưa có Commit nào được tạo.

Quy trình lúc này là:

```text
Working Tree
      │
      ▼
Index
      │
      ▼
Tree Object
```

---

### Plumbing Corner

Kiểm tra Tree vừa tạo:

```bash
git cat-file -p <tree-hash>
```

Bạn sẽ thấy:

```text
100644 blob ...
```

Tree chỉ chứa danh sách Blob và thư mục con.

---

## Lab 2 — Commit chứa những gì?

### Experiment

Tạo Commit:

```bash
git commit -m "Initial commit"
```

Lấy Hash:

```bash
git rev-parse HEAD
```

Sau đó:

```bash
git cat-file -p HEAD
```

---

### Observe

Ví dụ:

```text
tree ...

author ...

committer ...

Initial commit
```

---

### Explain

Commit không chứa:

- mã nguồn
- nội dung tệp

Commit chỉ tham chiếu tới:

- Tree
- Parent
- Metadata

Mã nguồn nằm trong Blob Object.

---

## Lab 3 — Blob được lưu thế nào?

### Experiment

Tạo Blob:

```bash
git hash-object app.txt
```

Tiếp tục:

```bash
git hash-object -w app.txt
```

---

### Observe

Git trả về Hash của Blob.

---

### Explain

`hash-object`

- tính Hash của nội dung
- với `-w`, ghi Blob vào Object Database

Đây là một trong những thao tác cơ bản nhất của Git.

---

### Plumbing Corner

Đọc Blob:

```bash
git cat-file -p <blob-hash>
```

Bạn sẽ thấy chính nội dung của tệp.

---

## Lab 4 — Branch thực chất là gì?

### Experiment

Tạo Branch:

```bash
git branch feature
```

Quan sát:

```bash
git rev-parse main

git rev-parse feature
```

---

### Observe

Hai Branch có cùng Hash.

---

### Explain

Branch chỉ là một Reference.

Không có dữ liệu nào được sao chép.

---

### Plumbing Corner

Đọc Reference:

```bash
git symbolic-ref HEAD
```

và:

```bash
cat .git/refs/heads/main
```

---

## Lab 5 — Git Log lấy dữ liệu từ đâu?

### Experiment

Thực hiện:

```bash
git log --oneline
```

Sau đó:

```bash
git rev-list HEAD
```

---

### Observe

Hai lệnh đều liệt kê Commit.

---

### Explain

`git log`

thực chất duyệt Commit Graph.

`git rev-list`

là Plumbing Command để duyệt các Commit có thể đi tới từ một Reference.

---

## Mapping

| Lệnh hằng ngày | Plumbing liên quan |
|----------------|--------------------|
| `git add` | `git hash-object`, `git update-index` |
| `git commit` | `git write-tree`, `git commit-tree`, `git update-ref` |
| `git branch` | `git update-ref` |
| `git switch` | `git symbolic-ref`, `git read-tree`, `git checkout-index` |
| `git log` | `git rev-list`, `git cat-file` |
| `git status` | Đọc Working Tree, Index và HEAD để so sánh trạng thái |

---

## Challenge

### Challenge 1

Thử tạo một Blob:

```bash
echo "Git Engineering Lab" > demo.txt

git hash-object -w demo.txt
```

Sau đó dùng:

```bash
git cat-file -p <blob-hash>
```

Giải thích vì sao Blob không lưu tên tệp.

---

### Challenge 2

Lấy Hash của HEAD:

```bash
git rev-parse HEAD
```

Tiếp tục:

```bash
git cat-file -p HEAD
```

Sau đó lấy Hash của Tree và đọc tiếp:

```bash
git cat-file -p <tree-hash>
```

Cuối cùng lấy Hash của Blob và đọc:

```bash
git cat-file -p <blob-hash>
```

Hãy mô tả đường đi:

```text
Commit
    │
    ▼
Tree
    │
    ▼
Blob
```

---

### Challenge 3

Thực hiện:

```bash
git switch -c experiment
```

Quan sát:

```bash
git rev-parse experiment

git rev-parse HEAD
```

Giải thích vì sao hai Hash giống nhau ngay sau khi tạo Branch.

---

## Takeaway

Sau bài Lab này, bạn cần ghi nhớ:

- Porcelain là giao diện dành cho người dùng.
- Plumbing là các thao tác cấp thấp của Git.
- Commit tham chiếu tới Tree, Tree tham chiếu tới Blob.
- Branch chỉ là Reference tới Commit.
- Nhiều lệnh Git quen thuộc thực chất là sự kết hợp của nhiều Plumbing Commands.
- Hiểu Plumbing giúp bạn giải thích được hầu hết hành vi của Git thay vì chỉ ghi nhớ lệnh.

---

## Related Handbook

- 02 — Git Object Database
- 03 — Working Tree, Index & HEAD
- 04 — Commit History & References
- 05 — Branch & Tag
- 08 — History Rewrite

---

## Kết luận

Bạn đã hoàn thành toàn bộ **Git Engineering Lab**.

Qua ba phần của repository:

- **Cheat Sheet** giúp bạn sử dụng Git trong công việc hằng ngày.
- **Handbook** giúp bạn hiểu mô hình và kiến trúc của Git.
- **Appendix** giúp bạn kiểm chứng mọi khái niệm bằng các thí nghiệm và các lệnh Plumbing.

Nếu gặp một hành vi "lạ" của Git trong tương lai, hãy thử trả lời ba câu hỏi:

1. HEAD đang tham chiếu tới đâu?
2. Reference nào vừa thay đổi?
3. Object nào vừa được tạo?

Chỉ với ba câu hỏi này, bạn sẽ giải thích được phần lớn những gì Git đang làm phía sau.
