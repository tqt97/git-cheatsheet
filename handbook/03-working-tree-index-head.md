# Working Tree, Index & HEAD

> Hiểu ba thành phần cốt lõi quyết định hầu hết mọi thao tác trong Git.

---

## Tổng quan

Hầu như mọi lệnh Git đều tác động đến một trong ba khu vực:

- Working Tree
- Index (Staging Area)
- HEAD

Nếu chưa hiểu ba thành phần này, các lệnh như:

- `git add`
- `git restore`
- `git reset`
- `git checkout`
- `git commit`

sẽ rất dễ gây nhầm lẫn.

Thực tế, các lệnh trên chỉ là những cách khác nhau để **di chuyển dữ liệu giữa ba khu vực này**.

---

## Mental Model

Hãy tưởng tượng Git giống như một dây chuyền sản xuất.

```text
           Bạn chỉnh sửa

                 │

                 ▼

        +----------------+
        | Working Tree   |
        +----------------+

                 │
           git add

                 ▼

        +----------------+
        |     Index      |
        | (Staging Area) |
        +----------------+

                 │
          git commit

                 ▼

        +----------------+
        |     HEAD       |
        | Latest Commit  |
        +----------------+
```

---

## Working Tree là gì?

Working Tree là nơi bạn làm việc mỗi ngày.

Ví dụ:

```text
project/

README.md

src/

main.py
```

Khi mở VS Code hoặc IntelliJ và sửa mã nguồn, bạn đang thay đổi Working Tree.

Working Tree:

- Có thể thay đổi liên tục.
- Có thể chưa được Git theo dõi.
- Chưa nằm trong lịch sử Repository.

---

## Index (Staging Area) là gì?

Index là khu vực trung gian giữa Working Tree và Commit.

Nó lưu danh sách các thay đổi sẽ xuất hiện trong Commit tiếp theo.

```text
Working Tree

↓

git add

↓

Index

↓

git commit
```

Đây là lý do Git có hai bước:

```bash
git add

git commit
```

Thay vì chỉ cần một lệnh.

---

## HEAD là gì?

HEAD là một Reference đặc biệt.

HEAD luôn chỉ tới Commit hiện tại.

Ví dụ:

```text
main

↓

Commit C

↑

HEAD
```

Nếu chuyển Branch:

```bash
git switch develop
```

HEAD sẽ chuyển sang Commit mới.

HEAD **không chứa dữ liệu**.

HEAD chỉ là Reference.

---

## Git Internals Diagram

```text
                Working Tree
                     │
                     │ chỉnh sửa file
                     ▼
          +----------------------+
          |     Working Tree     |
          +----------------------+
                     │
              git add
                     ▼
          +----------------------+
          |        Index         |
          |   (Staging Area)     |
          +----------------------+
                     │
            git commit
                     ▼
          +----------------------+
          |      Commit (HEAD)   |
          +----------------------+
                     │
             Branch Reference
                     │
                     ▼
                 Object Database
```

Đây là sơ đồ quan trọng nhất của Git.

---

## Data Flow

### Bước 1

Bạn sửa:

```text
README.md
```

```text
Working Tree

README.md (đã sửa)
```

Git chưa lưu gì.

---

### Bước 2

```bash
git add README.md
```

```text
Working Tree

↓

Index
```

Git sao chép nội dung hiện tại của tệp vào Index.

---

### Bước 3

```bash
git commit
```

```text
Index

↓

Tree

↓

Commit

↓

HEAD
```

Git tạo:

- Blob mới (nếu cần)
- Tree mới
- Commit mới

Sau đó Branch được cập nhật sang Commit mới.

---

## Inside Git

Giả sử:

```bash
echo "Version 1" > README.md
```

Working Tree:

```text
README

Version 1
```

Sau:

```bash
git add README.md
```

Working Tree

```text
Version 1
```

Index

```text
Version 1
```

---

Bạn tiếp tục sửa:

```text
Version 2
```

Lúc này:

Working Tree

```text
Version 2
```

Index

```text
Version 1
```

Đây là điều rất nhiều người mới không nhận ra.

Index và Working Tree có thể hoàn toàn khác nhau.

---

## Ví dụ thực tế

Giả sử:

```bash
git add README.md
```

Sau đó sửa tiếp:

```text
README.md
```

Lúc này:

```text
Working Tree

Version 2
```

```text
Index

Version 1
```

Nếu Commit ngay:

```bash
git commit
```

Commit sẽ chứa:

```text
Version 1
```

không phải Version 2.

Muốn Commit Version 2:

```bash
git add README.md
```

một lần nữa.

---

## HEAD luôn ở đâu?

```text
main

│

▼

Commit C

▲

HEAD
```

Sau Commit mới:

```text
main

│

▼

Commit D

▲

HEAD
```

HEAD luôn theo Branch hiện tại.

---

## Detached HEAD

Nếu thực hiện:

```bash
git checkout v1.0.0
```

hoặc

```bash
git switch --detach
```

sẽ có:

```text
HEAD

│

▼

Commit B
```

HEAD không còn trỏ tới Branch.

Đây gọi là **Detached HEAD**.

Nếu Commit trong trạng thái này:

```text
Commit B

↓

Commit C
```

Commit mới sẽ không thuộc Branch nào.

Muốn giữ lại:

```bash
git switch -c hotfix
```

---

## So sánh ba khu vực

| Thành phần | Vai trò | Có thể thay đổi? |
|------------|----------|------------------|
| Working Tree | Mã nguồn đang chỉnh sửa | Có |
| Index | Chuẩn bị Commit | Có |
| HEAD | Commit hiện tại | Không (chỉ thay đổi khi Commit hoặc Switch Branch) |

---

## Hiểu lầm phổ biến

### git add tạo Commit

❌ Sai.

`git add` chỉ cập nhật Index.

---

### git commit lấy dữ liệu từ Working Tree

❌ Sai.

Git chỉ Commit dữ liệu trong Index.

---

### HEAD là Branch

❌ Sai.

HEAD chỉ trỏ tới Branch hoặc trực tiếp tới một Commit.

---

### Working Tree luôn giống Index

❌ Sai.

Sau khi `git add`, bạn vẫn có thể tiếp tục chỉnh sửa Working Tree.

---

## Checklist ghi nhớ

✓ Working Tree là nơi chỉnh sửa mã nguồn.

✓ Index là nơi chuẩn bị Commit.

✓ Commit chỉ lấy dữ liệu từ Index.

✓ HEAD luôn trỏ tới Commit hiện tại.

✓ `git add` có thể cần chạy nhiều lần nếu tiếp tục sửa tệp.

✓ Hầu hết các lệnh Git đều là thao tác giữa Working Tree, Index và HEAD.

---

## Tóm tắt

Ba thành phần:

```text
Working Tree
      │
git add
      ▼
Index
      │
git commit
      ▼
HEAD
```

là nền tảng của gần như toàn bộ Git.

Khi hiểu rõ luồng dữ liệu này, bạn sẽ dễ dàng giải thích hành vi của:

- `git restore`
- `git reset`
- `git checkout`
- `git switch`
- `git commit`
- `git add`

mà không cần học thuộc lòng từng lệnh.

---

## Chương tiếp theo

→ [04. Commit History & References](./04-commit-history-and-refs.md)
