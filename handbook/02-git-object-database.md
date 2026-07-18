# Git Object Database

> Hiểu cách Git lưu trữ dữ liệu bên trong Repository.

---

## Tổng quan

Một trong những điểm khác biệt lớn nhất giữa Git và nhiều hệ thống quản lý phiên bản khác là **Git không lưu tệp (File), Git lưu Object**.

Mỗi khi tạo Commit, Git sẽ xây dựng một tập hợp các Object và lưu chúng trong **Object Database**.

Nhờ cơ chế này, Git có thể:

- Theo dõi lịch sử rất nhanh.
- Tiết kiệm dung lượng lưu trữ.
- Phát hiện thay đổi chính xác.
- Khôi phục dữ liệu ngay cả khi Branch hoặc Commit đã bị xóa.

Muốn hiểu Git hoạt động như thế nào, trước tiên cần hiểu Object Database.

---

## Mental Model

Hãy hình dung Repository giống như một cơ sở dữ liệu.

```text
Repository

│

├── Working Tree

├── Index

└── Object Database
```

Người dùng chỉ làm việc với Working Tree.

Git lưu mọi dữ liệu thực sự trong Object Database.

---

## Object Database nằm ở đâu?

Trong mọi Repository đều có thư mục:

```text
.git/
```

Bên trong:

```text
.git

├── objects/

├── refs/

├── HEAD

├── config

└── logs/
```

Trong đó:

```text
objects/
```

là nơi Git lưu toàn bộ dữ liệu.

---

## Git Object là gì?

Object là đơn vị lưu trữ cơ bản của Git.

Git có bốn loại Object:

| Object | Chức năng |
|---------|-----------|
| Blob | Lưu nội dung của tệp |
| Tree | Lưu cấu trúc thư mục |
| Commit | Lưu Snapshot của Repository |
| Tag | Lưu thông tin phiên bản |

Mọi Commit đều được tạo từ bốn loại Object này.

---

## Blob Object

### Khái niệm

Blob chỉ lưu **nội dung của tệp**.

Blob **không biết**:

- Tên tệp.
- Đường dẫn.
- Quyền truy cập.
- Thư mục chứa nó.

Ví dụ

```text
README.md

Hello Git
```

Git tạo:

```text
Blob

↓

"Hello Git"
```

Nếu đổi tên:

```text
README.md

↓

INTRODUCTION.md
```

Blob **không thay đổi**.

Vì nội dung vẫn giống nhau.

---

## Tree Object

Tree mô tả cấu trúc thư mục.

Ví dụ

```text
project/

README.md

src/

main.py
```

Git tạo:

```text
Tree

├── README.md

└── src
        │
        ▼
      Tree
          │
          ▼
       main.py
```

Tree không lưu nội dung.

Tree chỉ lưu:

- Tên.
- Kiểu Object.
- Hash.

---

## Commit Object

Commit không lưu mã nguồn.

Commit lưu thông tin về Snapshot.

Ví dụ

```text
Commit

│

├── Tree

├── Parent

├── Author

├── Committer

├── Timestamp

└── Message
```

Trong đó:

Tree chính là ảnh chụp của Repository tại thời điểm Commit.

---

## Tag Object

Annotated Tag cũng là một Object.

Tag lưu:

- Tên phiên bản.
- Người tạo.
- Thời gian.
- Commit được đánh dấu.

Đó là lý do Annotated Tag được khuyến nghị cho Release.

---

## Quan hệ giữa các Object

```text
Commit

│

▼

Tree

├── Blob

├── Blob

└── Tree
       │
       ▼
     Blob
```

Một Commit không chứa File.

Commit chỉ tham chiếu đến Tree.

Tree tiếp tục tham chiếu đến Blob hoặc Tree khác.

---

## Inside Git

Giả sử bạn thực hiện:

```bash
echo "Hello Git" > README.md

git add README.md

git commit -m "Initial commit"
```

Git sẽ thực hiện:

```text
README.md

↓

Blob

↓

Tree

↓

Commit

↓

Branch
```

Điều quan trọng là:

Branch **không chứa dữ liệu**.

Branch chỉ trỏ đến Commit mới nhất.

---

## Vì sao Git tiết kiệm dung lượng?

Giả sử:

Commit đầu tiên

```text
README.md
```

Commit thứ hai

```text
README.md

main.py
```

README không thay đổi.

Git **không tạo Blob mới**.

Blob cũ được tái sử dụng.

Ví dụ

```text
Commit A

│

├── Blob 1

└── Blob 2


Commit B

│

├── Blob 1

├── Blob 2

└── Blob 3
```

Blob 1 và Blob 2 được dùng chung.

---

## Hash trong Git

Mỗi Object có một mã định danh duy nhất.

Ví dụ

```text
4b825dc642cb6eb9a060e54bf8d69288fbee4904
```

Hash được tính từ:

- Nội dung Object.
- Loại Object.

Nếu nội dung thay đổi:

Hash cũng thay đổi.

Đó là lý do Git dễ dàng phát hiện thay đổi.

---

## Ví dụ thực tế

Bạn sửa:

```text
README.md
```

Sau đó:

```bash
git add

git commit
```

Git sẽ:

- Tạo Blob mới cho README.
- Tạo Tree mới.
- Tạo Commit mới.

Những Blob không thay đổi sẽ tiếp tục được sử dụng.

---

## Điều gì xảy ra khi đổi tên File?

Ví dụ

```text
README.md

↓

Guide.md
```

Nếu nội dung giữ nguyên:

Git:

- Không tạo Blob mới.
- Chỉ tạo Tree mới.
- Tạo Commit mới.

Đây là lý do Git không thực sự "theo dõi việc đổi tên tệp".

Git chỉ nhận thấy:

- Một Blob cũ.
- Một Tree mới tham chiếu tới Blob đó với tên khác.

---

## Hiểu lầm phổ biến

### Git lưu File

❌ Sai.

Git lưu Object.

---

### Commit chứa mã nguồn

❌ Sai.

Commit chứa tham chiếu tới Tree.

---

### Tree là thư mục thật

❌ Sai.

Tree chỉ là Object mô tả cấu trúc thư mục.

---

### Branch chứa dữ liệu

❌ Sai.

Branch chỉ là một Reference trỏ tới Commit.

---

## Tóm tắt

- Git lưu dữ liệu trong `.git/objects`.
- Git có bốn loại Object: Blob, Tree, Commit và Tag.
- Blob lưu nội dung tệp.
- Tree lưu cấu trúc thư mục.
- Commit lưu Snapshot thông qua Tree.
- Tag lưu thông tin phiên bản.
- Mọi Object đều được định danh bằng Hash.
- Branch không chứa dữ liệu, chỉ trỏ tới Commit.

---

## Chương tiếp theo

→ [03. Working Tree, Index & HEAD](./03-working-tree-index-head.md)