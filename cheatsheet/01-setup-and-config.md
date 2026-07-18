# Thiết lập & Cấu hình Git

> Thiết lập Git đúng cách trước khi tạo hoặc tham gia bất kỳ repository nào.

---

# Tổng quan

Trong bài viết này, bạn sẽ tìm hiểu cách:

- Kiểm tra Git đã được cài đặt hay chưa.
- Thiết lập thông tin người dùng (`user.name`, `user.email`).
- Xem và quản lý cấu hình Git.
- Hiểu phạm vi áp dụng của từng loại cấu hình (`system`, `global`, `local`).
- Khởi tạo một Git Repository mới.
- Quản lý các Remote Repository.

Đây là những bước cấu hình cơ bản nên thực hiện ngay sau khi cài đặt Git.

---

# Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git --version` | Kiểm tra phiên bản Git |
| `git config` | Đọc hoặc thay đổi cấu hình Git |
| `git config --global` | Thiết lập cấu hình cho người dùng hiện tại |
| `git config --system` | Thiết lập cấu hình cho toàn bộ hệ thống |
| `git config --local` | Thiết lập cấu hình cho Repository hiện tại |
| `git config --list` | Hiển thị toàn bộ cấu hình |
| `git init` | Khởi tạo Git Repository |
| `git remote` | Quản lý Remote Repository |

---

# Kiểm tra phiên bản Git

## Mục đích

Kiểm tra Git đã được cài đặt trên máy hay chưa và xác định phiên bản đang sử dụng.

## Cú pháp

```bash
git --version
```

## Ví dụ

```bash
git --version
```

Kết quả:

```text
git version 2.50.1
```

---

# Thiết lập tên người dùng

## Mục đích

Thiết lập tên tác giả sẽ được ghi trong mỗi Commit.

## Cú pháp

```bash
git config --global user.name "John Doe"
```

## Kiểm tra

```bash
git config --global user.name
```

---

# Thiết lập địa chỉ Email

## Mục đích

Thiết lập địa chỉ email sẽ được gắn với các Commit.

Thông thường:

- Sử dụng email công ty cho các dự án tại doanh nghiệp.
- Sử dụng email cá nhân cho các dự án cá nhân hoặc mã nguồn mở.

## Cú pháp

```bash
git config --global user.email "john@example.com"
```

## Kiểm tra

```bash
git config --global user.email
```

---

# Thiết lập tên nhánh mặc định

## Mục đích

Xác định tên nhánh mặc định khi tạo một Repository mới bằng `git init`.

## Cú pháp

```bash
git config --global init.defaultBranch main
```

Sau khi thiết lập, các Repository mới sẽ sử dụng `main` thay vì `master`.

---

# Thiết lập trình soạn thảo mặc định

## Mục đích

Chỉ định trình soạn thảo mà Git sẽ sử dụng khi cần nhập Commit Message hoặc chỉnh sửa nội dung.

### Visual Studio Code

```bash
git config --global core.editor "code --wait"
```

### Vim

```bash
git config --global core.editor "vim"
```

---

# Thiết lập Merge Tool

Ví dụ sử dụng Visual Studio Code làm công cụ hỗ trợ Merge.

```bash
git config --global merge.tool vscode
```

---

# Thiết lập Line Ending

Do Windows và Linux/macOS sử dụng ký tự xuống dòng khác nhau nên Git cung cấp tùy chọn để tự động chuyển đổi.

## Windows

```bash
git config --global core.autocrlf true
```

## macOS / Linux

```bash
git config --global core.autocrlf input
```

---

# Xem cấu hình hiện tại

Hiển thị toàn bộ cấu hình.

```bash
git config --list
```

Hiển thị một cấu hình cụ thể.

```bash
git config user.email
```

Hiển thị cấu hình kèm theo vị trí của file cấu hình.

```bash
git config --list --show-origin
```

---

# Phạm vi cấu hình

Git hỗ trợ ba cấp cấu hình.

| Phạm vi | Áp dụng cho |
|---------|-------------|
| `system` | Toàn bộ máy tính |
| `global` | Người dùng hiện tại |
| `local` | Repository hiện tại |

Thứ tự ưu tiên:

```
local
    ↓
global
    ↓
system
```

Nếu cùng một cấu hình được thiết lập ở nhiều cấp, Git sẽ ưu tiên sử dụng cấu hình có mức ưu tiên cao hơn.

---

# Khởi tạo Repository

## Mục đích

Tạo một Git Repository mới trong thư mục hiện tại.

## Cú pháp

```bash
git init
```

## Ví dụ

```bash
mkdir demo

cd demo

git init
```

Kết quả:

```text
demo/
└── .git/
```

Thư mục `.git` chứa toàn bộ dữ liệu và lịch sử của Repository.

---

# Đổi tên nhánh mặc định

Nếu Repository đang sử dụng `master`, có thể đổi sang `main` bằng lệnh:

```bash
git branch -M main
```

---

# Quản lý Remote Repository

## Xem danh sách Remote

```bash
git remote -v
```

---

## Thêm Remote

```bash
git remote add origin https://github.com/org/project.git
```

---

## Đổi tên Remote

```bash
git remote rename origin upstream
```

---

## Xóa Remote

```bash
git remote remove origin
```

---

# Các lỗi thường gặp

## Thiết lập sai Email

Sai:

```bash
git config user.email
```

Đúng:

```bash
git config --global user.email "john@example.com"
```

---

## Quên thiết lập tên nhánh mặc định

Một số dự án sử dụng `main`, trong khi Git cũ mặc định tạo nhánh `master`.

Điều này có thể gây khác biệt so với quy ước của nhóm.

---

## Thiết lập nhầm ở cấp `local`

Nếu sử dụng `--local`, cấu hình chỉ có hiệu lực trong Repository hiện tại.

Nhiều người vô tình nghĩ rằng cấu hình đã được áp dụng cho toàn bộ máy.

---

# Thực hành tốt

- Thiết lập Git ngay sau khi cài đặt.
- Sử dụng email phù hợp với từng môi trường làm việc.
- Đặt `main` làm nhánh mặc định nếu dự án không có quy ước khác.
- Kiểm tra lại cấu hình bằng `git config --list`.
- Thiết lập trình soạn thảo yêu thích trước khi bắt đầu làm việc.
- Hiểu rõ sự khác nhau giữa `system`, `global` và `local`.

---

# Lệnh liên quan

- `git init`
- `git clone`
- `git remote`
- `git config`

---

# Bài tiếp theo

→ [02. Tạo và sao chép Repository](./02-create-and-clone.md)