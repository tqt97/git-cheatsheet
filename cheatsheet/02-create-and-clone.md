# Create & Clone

> Tạo một Git Repository mới hoặc sao chép (Clone) một Repository đã có sẵn.

---

# Tổng quan

Trong bài viết này, bạn sẽ tìm hiểu cách:

- Khởi tạo một Git Repository mới.
- Sao chép một Repository từ Remote Repository về máy.
- Clone vào một thư mục cụ thể.
- Clone một nhánh (Branch) cụ thể.
- Clone với lịch sử rút gọn (Shallow Clone).
- Quản lý Remote sau khi Clone.

Đây là những thao tác đầu tiên khi bắt đầu một dự án mới hoặc tham gia vào một dự án đã tồn tại.

---

# Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git init` | Khởi tạo Git Repository |
| `git clone` | Sao chép Repository từ Remote |
| `git clone --depth` | Clone với lịch sử rút gọn |
| `git clone --branch` | Clone một Branch cụ thể |
| `git remote` | Xem danh sách Remote |
| `git remote add` | Thêm Remote mới |

---

# Tạo Repository mới

## Mục đích

Khởi tạo một Git Repository trong thư mục hiện tại.

Sau khi thực hiện, Git sẽ tạo thư mục `.git` để lưu trữ toàn bộ lịch sử và dữ liệu của Repository.

## Cú pháp

```bash
git init
```

## Ví dụ

```bash
mkdir my-app

cd my-app

git init
```

Kết quả:

```text
my-app/
└── .git/
```

---

# Clone Repository

## Mục đích

Sao chép toàn bộ Repository từ Remote Repository về máy tính.

Repository sau khi Clone sẽ bao gồm:

- Toàn bộ lịch sử Commit.
- Các Branch.
- Remote mặc định (`origin`).
- Nội dung của dự án.

## Cú pháp

```bash
git clone https://github.com/org/project.git
```

Kết quả:

```text
project/
```

---

# Clone vào thư mục chỉ định

## Mục đích

Clone Repository nhưng lưu vào một thư mục có tên khác với tên Repository.

## Cú pháp

```bash
git clone https://github.com/org/project.git backend
```

Kết quả:

```text
backend/
```

---

# Clone một Branch cụ thể

## Mục đích

Chỉ Clone và checkout một Branch xác định thay vì Branch mặc định.

## Cú pháp

```bash
git clone --branch develop https://github.com/org/project.git
```

Phù hợp khi bạn chỉ làm việc với một Branch cụ thể.

---

# Shallow Clone

## Mục đích

Clone Repository nhưng chỉ tải một số lượng Commit gần nhất thay vì toàn bộ lịch sử.

## Cú pháp

```bash
git clone --depth 1 https://github.com/org/project.git
```

`--depth 1` chỉ tải Commit mới nhất.

## Khi nào nên sử dụng

- Pipeline CI/CD.
- Kiểm thử tự động.
- Repository có lịch sử rất lớn.
- Không cần tra cứu lịch sử Commit.

> **Lưu ý:** Shallow Clone không phù hợp nếu bạn cần xem đầy đủ lịch sử hoặc thực hiện các thao tác như `rebase` hay `bisect`.

---

# Xem danh sách Remote

## Mục đích

Hiển thị các Remote Repository đã được cấu hình.

## Cú pháp

```bash
git remote -v
```

Ví dụ:

```text
origin  https://github.com/org/project.git (fetch)
origin  https://github.com/org/project.git (push)
```

---

# Thêm Remote

## Mục đích

Thêm một Remote Repository mới vào Repository hiện tại.

## Cú pháp

```bash
git remote add origin https://github.com/org/project.git
```

Sau khi thêm, bạn có thể sử dụng các lệnh như:

```bash
git fetch

git pull

git push
```

---

# Kiểm tra Remote

Sau khi thêm hoặc Clone Repository, nên kiểm tra lại danh sách Remote.

```bash
git remote -v
```

Đảm bảo URL của `origin` chính xác trước khi thực hiện `push`.

---

# Các lỗi thường gặp

## Khởi tạo Repository bên trong một Repository khác

Thực hiện `git init` trong một thư mục đã có `.git` sẽ tạo ra cấu trúc Repository lồng nhau, gây khó khăn trong việc quản lý mã nguồn.

---

## Clone vào thư mục đã tồn tại

Nếu thư mục đích đã tồn tại và không rỗng, lệnh `git clone` sẽ báo lỗi.

---

## Quên kiểm tra Remote

Một số trường hợp Clone từ Fork hoặc Mirror khiến `origin` không trỏ đến Repository mong muốn.

Luôn kiểm tra lại bằng:

```bash
git remote -v
```

---

## Sử dụng Shallow Clone không đúng mục đích

`--depth` giúp giảm thời gian tải mã nguồn nhưng sẽ giới hạn lịch sử Commit.

Nếu cần xem lịch sử hoặc thực hiện các thao tác nâng cao, nên Clone đầy đủ.

---

# Thực hành tốt

- Ưu tiên sử dụng `git clone` khi làm việc với dự án đã tồn tại.
- Chỉ sử dụng `git init` khi tạo một Repository mới.
- Kiểm tra Remote ngay sau khi Clone.
- Chỉ sử dụng `--depth` khi thực sự không cần toàn bộ lịch sử Commit.
- Đặt tên thư mục Clone rõ ràng nếu làm việc với nhiều phiên bản của cùng một dự án.

---

# Lệnh liên quan

- `git remote`
- `git fetch`
- `git pull`
- `git push`

---

# Bài tiếp theo

→ [03. Inspect Status & History](./03-inspect-status-and-history.md)