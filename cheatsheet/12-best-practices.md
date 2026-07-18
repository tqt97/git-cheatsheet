# Best Practices

> Các nguyên tắc và kinh nghiệm giúp sử dụng Git hiệu quả, an toàn và dễ cộng tác trong các dự án thực tế.

---

## Tổng quan

Git không chỉ là tập hợp các câu lệnh, mà còn là một quy trình làm việc (Workflow).

Việc sử dụng đúng các lệnh mới chỉ là bước đầu. Để Repository luôn rõ ràng, dễ bảo trì và thuận tiện cho việc cộng tác, cần tuân thủ các nguyên tắc làm việc thống nhất.

Tài liệu này tổng hợp những Best Practices được áp dụng phổ biến trong các dự án mã nguồn mở và các nhóm phát triển phần mềm chuyên nghiệp.

---

## Mục tiêu

Một Repository được quản lý tốt cần đảm bảo:

- Lịch sử Commit rõ ràng.
- Dễ tìm kiếm thay đổi.
- Dễ Review.
- Dễ khôi phục khi có sự cố.
- Hạn chế Merge Conflict.
- Thuận tiện cho nhiều người cùng phát triển.

---

## Quy trình làm việc được khuyến nghị

```text
git switch main
        │
        ▼
git pull --rebase
        │
        ▼
git switch -c feature/login
        │
        ▼
Phát triển tính năng
        │
        ▼
git add
        │
        ▼
git commit
        │
        ▼
git fetch
        │
        ▼
git rebase main
        │
        ▼
Giải quyết Conflict (nếu có)
        │
        ▼
git push
        │
        ▼
Tạo Pull Request
        │
        ▼
Code Review
        │
        ▼
Merge vào main
```

---

## Luôn làm việc trên Feature Branch

Không nên phát triển trực tiếp trên:

```text
main
```

Thay vào đó:

```bash
git switch -c feature/login
```

Mỗi Branch chỉ nên phục vụ một mục tiêu cụ thể:

- Một tính năng.
- Một lỗi cần sửa.
- Một công việc riêng biệt.

Điều này giúp:

- Dễ Review.
- Dễ Merge.
- Dễ Revert.
- Hạn chế xung đột.

---

## Đặt tên Branch có ý nghĩa

Nên sử dụng quy ước thống nhất.

Ví dụ:

```text
feature/login

feature/user-profile

bugfix/payment

hotfix/security

release/v2.0.0

docs/update-readme

refactor/auth-service

test/login-api
```

Không nên:

```text
abc

test

new

fix

branch1
```

---

## Commit nhỏ và tập trung

Một Commit chỉ nên giải quyết **một vấn đề**.

Ví dụ:

Tốt

```text
Add login API

Fix authentication timeout

Update API documentation
```

Không tốt

```text
Update project

Fix bug and update UI and modify database
```

Commit nhỏ giúp:

- Dễ Review.
- Dễ Revert.
- Dễ Debug.
- Dễ tìm lỗi.

---

## Viết Commit Message rõ ràng

Nên sử dụng động từ ở thì hiện tại.

Ví dụ:

```text
Add login endpoint

Fix validation error

Update README

Remove unused files
```

Nếu dự án sử dụng **Conventional Commits**:

```text
feat: add login endpoint

fix: resolve authentication timeout

docs: update installation guide

refactor: simplify user service

test: add unit tests for login

chore: update dependencies
```

---

## Kiểm tra trước khi Commit

Trước mỗi Commit, nên thực hiện:

```bash
git status
```

```bash
git diff
```

```bash
git diff --staged
```

Việc này giúp:

- Phát hiện tệp không mong muốn.
- Kiểm tra chính xác nội dung sẽ được Commit.
- Tránh đưa nhầm thông tin nhạy cảm vào Repository.

---

## Sử dụng `.gitignore`

Không nên Commit:

- File Build.
- File Log.
- File Cache.
- Thư mục IDE.
- File cấu hình cá nhân.
- Secret hoặc API Key.

Ví dụ:

```text
node_modules/

dist/

build/

.env

.vscode/

.idea/

*.log
```

---

## Đồng bộ thường xuyên

Không nên làm việc quá lâu mà không đồng bộ với Repository.

Thực hiện định kỳ:

```bash
git fetch
```

hoặc

```bash
git pull --rebase
```

Việc đồng bộ thường xuyên giúp giảm nguy cơ Merge Conflict.

---

## Rebase đúng cách

Chỉ nên Rebase:

- Branch cá nhân.
- Feature Branch chưa được chia sẻ.

Không nên Rebase:

- `main`
- `develop`
- Branch mà nhiều người đang cùng làm việc.

---

## Không Force Push tùy tiện

Không nên:

```bash
git push --force
```

Nếu thực sự cần:

```bash
git push --force-with-lease
```

Điều này giúp tránh ghi đè thay đổi của người khác.

---

## Thường xuyên Push lên Remote

Không nên giữ quá nhiều Commit chỉ ở Local.

Push định kỳ giúp:

- Sao lưu công việc.
- Dễ cộng tác.
- Giảm nguy cơ mất dữ liệu.

---

## Review trước khi Merge

Trước khi Merge vào Branch chính, cần kiểm tra:

- Chức năng hoạt động đúng.
- Không còn Conflict.
- Đã cập nhật từ `main`.
- Đã vượt qua kiểm thử.
- Commit Message rõ ràng.
- Không chứa mã thử nghiệm hoặc tệp không cần thiết.

---

## Quản lý Release bằng Tag

Không đánh dấu phiên bản bằng tên Branch.

Sử dụng:

```bash
git tag -a v1.2.0 -m "Release version 1.2.0"
```

Điều này giúp:

- Quản lý phiên bản.
- Quay lại Release cũ.
- Triển khai chính xác.

---

## Xử lý Merge Conflict

Khi xảy ra Conflict:

1. Đọc kỹ từng phần bị xung đột.
2. Hiểu nguyên nhân.
3. Chỉnh sửa thủ công.
4. Chạy lại kiểm thử.
5. Commit sau khi xác nhận mọi thứ hoạt động.

Không nên chọn một phía chỉ vì muốn hoàn thành nhanh.

---

## Dọn dẹp Repository

Sau khi Merge:

```bash
git branch -d feature/login
```

Định kỳ:

- Xóa Branch không còn sử dụng.
- Xóa Stash cũ.
- Xóa Tag thử nghiệm.
- Dọn các Remote Branch đã bị xóa.

Ví dụ:

```bash
git remote prune origin
```

---

## Sao lưu trước các thao tác nguy hiểm

Trước khi:

- Rebase.
- Reset.
- Force Push.
- Xóa Branch.

Nên:

```bash
git branch backup
```

Hoặc tạo Tag:

```bash
git tag backup-before-rebase
```

---

## Những điều không nên làm

- Commit trực tiếp lên `main`.
- Commit nhiều tính năng trong một Commit.
- Push mã chưa kiểm thử.
- Force Push lên Branch dùng chung.
- Commit Secret hoặc mật khẩu.
- Bỏ qua Code Review.
- Rebase lịch sử đã chia sẻ.
- Đặt tên Branch và Commit không có ý nghĩa.

---

## Checklist trước khi Push

```text
✓ Đang ở đúng Branch

✓ git status sạch

✓ Đã kiểm tra git diff

✓ Commit Message rõ ràng

✓ Đã Pull hoặc Fetch

✓ Đã giải quyết Conflict

✓ Đã chạy kiểm thử

✓ Không chứa Secret

✓ Không còn file tạm

✓ Push đúng Remote
```

---

## Tóm tắt các nguyên tắc quan trọng

| Nguyên tắc | Lợi ích |
|------------|---------|
| Mỗi Feature một Branch | Dễ quản lý và Review |
| Commit nhỏ | Dễ theo dõi lịch sử |
| Commit Message rõ ràng | Dễ tìm kiếm và bảo trì |
| Đồng bộ thường xuyên | Giảm Merge Conflict |
| Không Rebase lịch sử đã chia sẻ | Tránh xung đột |
| Ưu tiên `--force-with-lease` | An toàn hơn khi Force Push |
| Dùng Annotated Tag cho Release | Quản lý phiên bản tốt hơn |
| Luôn kiểm tra trước khi Push | Hạn chế sai sót |

---

## Kết luận

Git là công cụ quản lý phiên bản mạnh mẽ, nhưng hiệu quả của Git phụ thuộc nhiều vào quy trình làm việc của nhóm hơn là số lượng lệnh bạn biết.

Việc tuân thủ các Best Practices sẽ giúp:

- Repository có lịch sử rõ ràng.
- Dễ cộng tác giữa nhiều thành viên.
- Giảm rủi ro mất dữ liệu.
- Đơn giản hóa quá trình Review và bảo trì.
- Nâng cao chất lượng của toàn bộ dự án.

Hãy xem Git không chỉ là công cụ lưu mã nguồn, mà là công cụ quản lý lịch sử phát triển của dự án.

---

## Lệnh liên quan

- `git status`
- `git add`
- `git commit`
- `git switch`
- `git merge`
- `git rebase`
- `git fetch`
- `git pull`
- `git push`
- `git tag`
- `git stash`
- `git restore`

---
