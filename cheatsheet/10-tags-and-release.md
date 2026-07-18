# Tags & Release

> Đánh dấu các phiên bản quan trọng và quản lý Release trong Git.

---

## Tổng quan

Trong quá trình phát triển phần mềm, không phải mọi Commit đều đại diện cho một phiên bản phát hành.

Git cung cấp **Tag** để đánh dấu các Commit quan trọng như:

- Phiên bản phát hành (Release).
- Mốc triển khai (Deployment).
- Phiên bản thử nghiệm (Beta, RC).
- Các cột mốc của dự án.

Khác với Branch, **Tag là một tham chiếu cố định đến một Commit**. Sau khi được tạo, Tag thường không thay đổi.

---

## Tag là gì?

Tag là một tên (Label) gắn với một Commit cụ thể.

Ví dụ:

```text
A ---- B ---- C ---- D ---- E (main)
              ▲
              │
            v1.0.0
```

Sau này, dù Branch `main` tiếp tục phát triển, Tag `v1.0.0` vẫn luôn trỏ tới Commit `C`.

---

## Danh sách lệnh

| Lệnh | Mục đích |
|------|----------|
| `git tag` | Hiển thị danh sách Tag |
| `git tag <tag>` | Tạo Lightweight Tag |
| `git tag -a` | Tạo Annotated Tag |
| `git show <tag>` | Xem thông tin Tag |
| `git tag -d` | Xóa Tag cục bộ |
| `git push origin <tag>` | Push một Tag |
| `git push origin --tags` | Push tất cả Tag |
| `git push origin --delete` | Xóa Tag trên Remote |
| `git checkout <tag>` | Chuyển đến Commit của Tag |
| `git switch -c` | Tạo Branch từ Tag |

---

## Lightweight Tag

### Mục đích

Tạo một Tag đơn giản chỉ chứa tên tham chiếu đến Commit.

### Cú pháp

```bash
git tag v1.0.0
```

Ví dụ

```bash
git tag v1.0.0
```

Lightweight Tag không lưu:

- Người tạo.
- Ngày tạo.
- Thông điệp mô tả.

Phù hợp cho mục đích cá nhân hoặc đánh dấu tạm thời.

---

## Annotated Tag

### Mục đích

Tạo Tag có đầy đủ thông tin.

Đây là loại Tag được khuyến nghị cho các bản Release.

### Cú pháp

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Ví dụ

```bash
git tag -a v2.3.0 -m "Release version 2.3.0"
```

Annotated Tag lưu:

- Tên Tag.
- Người tạo.
- Thời gian tạo.
- Thông điệp.
- Commit được đánh dấu.

---

## Xem danh sách Tag

### Cú pháp

```bash
git tag
```

Ví dụ

```text
v1.0.0
v1.1.0
v2.0.0
```

---

## Xem thông tin Tag

### Cú pháp

```bash
git show v1.0.0
```

Ví dụ

```text
tag v1.0.0
Tagger: Nguyen Van A

Release version 1.0.0
```

Nếu là Annotated Tag, Git sẽ hiển thị thêm Commit tương ứng.

---

## Tạo Tag cho Commit cũ

### Mục đích

Đánh dấu một Commit bất kỳ trong lịch sử.

### Cú pháp

```bash
git tag -a v1.0.0 <commit>
```

Ví dụ

```bash
git tag -a v1.0.0 a13b7d2
```

---

## Push Tag lên Remote

Tag không được Push cùng với Commit.

Cần Push riêng.

### Push một Tag

```bash
git push origin v1.0.0
```

---

### Push toàn bộ Tag

```bash
git push origin --tags
```

---

## Xóa Tag

### Xóa Tag cục bộ

```bash
git tag -d v1.0.0
```

---

### Xóa Tag trên Remote

```bash
git push origin --delete v1.0.0
```

---

## Làm việc với Tag

### Chuyển đến một Tag

```bash
git checkout v1.0.0
```

Git sẽ chuyển sang trạng thái **Detached HEAD**.

Điều này có nghĩa là bạn không làm việc trên bất kỳ Branch nào.

---

## Tạo Branch từ Tag

Nếu muốn tiếp tục phát triển từ một phiên bản đã phát hành:

```bash
git switch -c hotfix/v1.0.0 v1.0.0
```

Ví dụ:

```text
A ---- B ---- C ---- D (main)
              ▲
            v1.0.0
              │
              ▼
      hotfix/v1.0.0
```

---

## Quy ước đặt tên Tag

Phổ biến nhất là **Semantic Versioning (SemVer)**.

Cấu trúc:

```text
MAJOR.MINOR.PATCH
```

Ví dụ:

```text
v1.0.0
v1.2.0
v1.2.3
v2.0.0
```

Ý nghĩa:

| Thành phần | Khi nào tăng phiên bản |
|------------|------------------------|
| MAJOR | Có thay đổi không tương thích với phiên bản trước |
| MINOR | Thêm tính năng nhưng vẫn tương thích |
| PATCH | Sửa lỗi, không thay đổi chức năng |

Ví dụ:

```text
v1.4.2
```

- MAJOR = 1
- MINOR = 4
- PATCH = 2

---

## Quy trình Release

```text
Hoàn thành tính năng
          │
          ▼
     Kiểm thử
          │
          ▼
     Merge vào main
          │
          ▼
   Tạo Annotated Tag
          │
          ▼
 git push origin main
          │
          ▼
git push origin v1.2.0
          │
          ▼
      Tạo Release
```

---

## Khi nào nên tạo Tag?

Nên tạo Tag khi:

- Phát hành phiên bản mới.
- Hoàn thành Sprint hoặc Milestone.
- Chuẩn bị triển khai lên Production.
- Muốn đánh dấu một Commit quan trọng.

Không nên tạo Tag cho mọi Commit.

---

## Các lỗi thường gặp

### Quên Push Tag

Nhiều người nghĩ:

```bash
git push
```

sẽ Push luôn Tag.

Thực tế, Tag phải được Push riêng.

---

### Sửa Tag đã chia sẻ

Việc thay đổi hoặc xóa Tag đã được sử dụng có thể gây nhầm lẫn cho các thành viên khác.

Nếu cần thay đổi, hãy thống nhất với nhóm trước.

---

### Commit trực tiếp khi đang ở Detached HEAD

Sau khi:

```bash
git checkout v1.0.0
```

mọi Commit mới sẽ không thuộc Branch nào.

Nếu muốn phát triển tiếp, hãy tạo Branch mới.

---

### Dùng Lightweight Tag cho Release

Đối với các phiên bản phát hành, nên sử dụng **Annotated Tag** để lưu đầy đủ thông tin.

---

## Thực hành tốt

- Sử dụng **Annotated Tag** cho tất cả các bản Release.
- Tuân thủ quy ước **Semantic Versioning**.
- Push Tag ngay sau khi tạo.
- Không chỉnh sửa hoặc xóa Tag đã được công bố nếu không thực sự cần thiết.
- Tạo Branch từ Tag khi cần phát triển hoặc sửa lỗi cho một phiên bản cũ.

---

## Lệnh liên quan

- `git branch`
- `git switch`
- `git log`
- `git show`
- `git push`

---

## Bài tiếp theo

→ [11. Decision Matrix](./11-decision-matrix.md)
