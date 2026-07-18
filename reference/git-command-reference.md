# Git Command Reference

> Danh sách các câu lệnh Git phổ biến được phân loại theo nhóm chức năng để tra cứu nhanh.

> **Lưu ý**
>
> - Đây là tài liệu tra cứu nhanh (Reference).
> - Giải thích chi tiết được trình bày trong **Cheat Sheet** và **Handbook**.
> - Không phải mọi lệnh Git đều được liệt kê, chỉ bao gồm những lệnh phổ biến và hữu ích trong thực tế.

---

## Setup & Config

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git --version` | - | Kiểm tra phiên bản Git | Xác minh môi trường |
| `git config --global user.name "<name>"` | Local Repository | Cấu hình tên người dùng | Thiết lập ban đầu |
| `git config --global user.email "<email>"` | Local Repository | Cấu hình email | Thiết lập ban đầu |
| `git config --list` | Local Repository | Hiển thị cấu hình | Kiểm tra cấu hình |
| `git config --global core.editor "<editor>"` | Local Repository | Thiết lập editor mặc định | Cá nhân hóa môi trường |
| `git config --global init.defaultBranch main` | Local Repository | Đặt Branch mặc định | Thiết lập môi trường |

---

## Repository

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git init` | Local Repository | Khởi tạo Repository | Bắt đầu project |
| `git clone <url>` | Local Repository + Remote | Clone Repository | Lấy mã nguồn |
| `git clone --depth 1 <url>` | Local Repository + Remote | Clone với lịch sử rút gọn | CI/CD hoặc project lớn |

---

## Inspect

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git status` | Working Tree + Index + Local Repository | Kiểm tra trạng thái Repository | Trước khi Commit |
| `git diff` | Working Tree + Index | So sánh thay đổi chưa Stage | Kiểm tra nội dung chỉnh sửa |
| `git diff --staged` | Index + Local Repository | So sánh thay đổi đã Stage | Trước khi Commit |
| `git show` | Local Repository | Hiển thị chi tiết Commit hoặc Object | Review Commit |
| `git log` | Local Repository | Xem lịch sử Commit | Tra cứu lịch sử |
| `git log --oneline` | Local Repository | Hiển thị lịch sử ngắn gọn | Hằng ngày |
| `git log --graph --decorate --oneline --all` | Local Repository | Hiển thị Commit Graph | Debug Branch |
| `git blame <file>` | Working Tree + Local Repository | Xem ai sửa từng dòng | Điều tra thay đổi |

---

## Stage & Commit

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git add <file>` | Index | Stage một tệp | Chuẩn bị Commit |
| `git add .` | Index | Stage toàn bộ thay đổi | Commit nhiều tệp |
| `git restore --staged <file>` | Index | Bỏ Stage | Sửa nhầm khi Stage |
| `git commit -m "<message>"` | Index + Local Repository | Tạo Commit | Lưu thay đổi |
| `git commit --amend` | Local Repository | Sửa Commit cuối | Chỉnh Message hoặc bổ sung thay đổi |
| `git commit --amend --no-edit` | Local Repository | Bổ sung vào Commit cuối | Quên Stage tệp |

---

## Branch

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git branch` | Local Repository | Liệt kê Branch | Kiểm tra Branch |
| `git branch <name>` | Local Repository | Tạo Branch | Chuẩn bị phát triển |
| `git switch <branch>` | Working Tree + Index + Local Repository | Chuyển Branch | Làm việc hằng ngày |
| `git switch -c <branch>` | Working Tree + Index + Local Repository | Tạo và chuyển Branch | Bắt đầu Feature |
| `git branch -d <branch>` | Local Repository | Xóa Branch đã Merge | Dọn dẹp |
| `git branch -D <branch>` | Local Repository | Buộc xóa Branch | Xóa Branch chưa Merge |

---

## Remote

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git remote -v` | Local Repository | Hiển thị Remote | Kiểm tra Remote |
| `git remote add origin <url>` | Local Repository | Thêm Remote | Thiết lập Repository |
| `git fetch` | Local Repository + Remote | Đồng bộ Object từ Remote | Trước Merge/Rebase |
| `git pull` | Working Tree + Index + Local Repository + Remote | Fetch và Merge | Đồng bộ mã nguồn |
| `git pull --rebase` | Working Tree + Index + Local Repository + Remote | Fetch và Rebase | Đồng bộ Branch cá nhân |
| `git push` | Local Repository + Remote | Đẩy Commit lên Remote | Hoàn thành công việc |
| `git push -u origin <branch>` | Local Repository + Remote | Push và thiết lập Tracking Branch | Push lần đầu |
| `git push --force-with-lease` | Local Repository + Remote | Ghi đè lịch sử an toàn hơn | Sau Rebase |

---

## Merge & Rebase

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git merge <branch>` | Working Tree + Index + Local Repository | Hợp nhất Branch | Hoàn thành Feature |
| `git merge --no-ff <branch>` | Local Repository | Luôn tạo Merge Commit | Giữ lịch sử rõ ràng |
| `git rebase <branch>` | Local Repository | Phát lại Commit trên nền mới | Đồng bộ Feature |
| `git rebase -i <commit>` | Local Repository | Interactive Rebase | Dọn dẹp lịch sử |
| `git cherry-pick <commit>` | Local Repository | Áp dụng một Commit | Chọn lọc thay đổi |

---

## Undo & Recovery

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git restore <file>` | Working Tree | Khôi phục thay đổi chưa Stage | Hoàn tác chỉnh sửa |
| `git restore --source=<commit> <file>` | Working Tree + Local Repository | Khôi phục tệp từ Commit | Lấy lại phiên bản cũ |
| `git reset --soft HEAD~1` | Local Repository | Di chuyển HEAD, giữ Index | Gộp Commit |
| `git reset HEAD~1` | Index + Local Repository | Di chuyển HEAD, giữ Working Tree | Chỉnh sửa Commit |
| `git reset --hard HEAD~1` | Working Tree + Index + Local Repository | Hoàn tác hoàn toàn | Hủy thay đổi cục bộ |
| `git revert <commit>` | Local Repository | Tạo Commit đảo ngược | Hoàn tác trên Branch chia sẻ |
| `git reflog` | Local Repository | Xem lịch sử HEAD | Khôi phục Commit |

---

## Stash

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git stash` | Working Tree + Index | Lưu tạm thay đổi | Chuyển việc gấp |
| `git stash list` | Local Repository | Liệt kê Stash | Quản lý Stash |
| `git stash pop` | Working Tree + Index | Khôi phục và xóa Stash | Tiếp tục công việc |
| `git stash apply` | Working Tree + Index | Khôi phục nhưng giữ Stash | Dùng lại nhiều lần |
| `git stash drop` | Local Repository | Xóa một Stash | Dọn dẹp |
| `git stash clear` | Local Repository | Xóa toàn bộ Stash | Dọn dẹp |

---

## Tag

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git tag` | Local Repository | Liệt kê Tag | Kiểm tra Release |
| `git tag <name>` | Local Repository | Tạo Lightweight Tag | Đánh dấu phiên bản |
| `git tag -a <name> -m "<message>"` | Local Repository | Tạo Annotated Tag | Release chính thức |
| `git push origin --tags` | Local Repository + Remote | Push tất cả Tag | Phát hành |

---

## History

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git log --stat` | Local Repository | Thống kê thay đổi theo Commit | Review |
| `git log --grep="<text>"` | Local Repository | Tìm Commit theo Message | Tra cứu |
| `git log --author="<name>"` | Local Repository | Lọc theo Author | Điều tra |
| `git shortlog` | Local Repository | Thống kê Commit theo Author | Báo cáo |

---

## File Operations

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git mv <old> <new>` | Working Tree + Index | Đổi tên hoặc di chuyển tệp | Refactor |
| `git rm <file>` | Working Tree + Index | Xóa tệp khỏi Repository | Xóa mã nguồn |
| `git rm --cached <file>` | Index | Bỏ theo dõi nhưng giữ tệp | Cập nhật `.gitignore` |

---

## Maintenance

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git clean -fd` | Working Tree | Xóa tệp chưa được theo dõi | Dọn Working Tree |
| `git gc` | Local Repository | Dọn dẹp Object Database | Bảo trì Repository |
| `git fsck` | Local Repository | Kiểm tra tính toàn vẹn Repository | Debug |
| `git count-objects -v` | Local Repository | Thống kê Object | Phân tích Repository |

---

## Internals

| Command | Scope | Purpose | Common Usage |
|---------|-------|----------|--------------|
| `git cat-file -p <object>` | Local Repository | Xem nội dung Object | Phân tích Object |
| `git cat-file -t <object>` | Local Repository | Xem loại Object | Kiểm tra Object |
| `git ls-tree <tree>` | Local Repository | Xem Tree Object | Phân tích Tree |
| `git rev-parse HEAD` | Local Repository | Phân giải Reference thành Hash | Debug Reference |
| `git symbolic-ref HEAD` | Local Repository | Xem Symbolic Reference | Kiểm tra HEAD |
| `git hash-object <file>` | Local Repository | Tính Hash của Blob | Học Object Database |
| `git hash-object -w <file>` | Local Repository | Ghi Blob vào Object Database | Thử nghiệm Internals |
| `git update-ref` | Local Repository | Cập nhật Reference | Thao tác Reference |
| `git write-tree` | Local Repository | Tạo Tree từ Index | Thử nghiệm Commit |
| `git commit-tree` | Local Repository | Tạo Commit từ Tree | Tìm hiểu Commit |

---

## Scope Legend

| Scope | Meaning |
|--------|---------|
| **Working Tree** | Ảnh hưởng các tệp đang làm việc |
| **Index** | Ảnh hưởng Staging Area |
| **Local Repository** | Ảnh hưởng Commit, Branch, Tag và Object Database |
| **Remote** | Giao tiếp với Remote Repository |