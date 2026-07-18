# Decision Matrix

> Hướng dẫn lựa chọn đúng lệnh Git cho từng tình huống thực tế.

---

## Tổng quan

Git cung cấp rất nhiều lệnh có chức năng gần giống nhau.

Ví dụ:

- `merge` hay `rebase`?
- `reset` hay `revert`?
- `restore` hay `checkout`?
- `fetch` hay `pull`?
- `stash` hay `commit`?

Việc chọn sai lệnh có thể dẫn đến:

- Mất dữ liệu.
- Thay đổi lịch sử Commit.
- Gây xung đột với đồng đội.
- Làm Repository trở nên khó quản lý.

Bài viết này tổng hợp các tình huống phổ biến và gợi ý lệnh phù hợp.

---

## Decision Matrix

| Tình huống | Nên dùng | Không nên dùng |
|------------|----------|----------------|
| Hủy thay đổi của một tệp | `git restore` | `git reset --hard` |
| Bỏ tệp khỏi Staging | `git restore --staged` | `git reset --hard` |
| Hoàn tác Commit chưa Push | `git reset` | `git revert` |
| Hoàn tác Commit đã Push | `git revert` | `git reset` |
| Đồng bộ Branch | `git pull --rebase` hoặc `git pull` | Chỉ `git fetch` nếu muốn cập nhật ngay |
| Tải dữ liệu mới nhưng chưa muốn cập nhật | `git fetch` | `git pull` |
| Chuyển công việc tạm thời | `git stash` | Commit tạm không cần thiết |
| Chỉ lấy một Commit | `git cherry-pick` | `git merge` |
| Gộp toàn bộ Branch | `git merge` | Cherry-pick nhiều Commit |
| Dọn lịch sử Branch cá nhân | `git rebase` | `git merge` |
| Đánh dấu phiên bản Release | `git tag -a` | Lightweight Tag |
| Khôi phục Commit bị mất | `git reflog` | Tạo lại bằng tay |

---

## Restore hay Reset?

### Tình huống

Bạn sửa nhầm một tệp và muốn quay về trạng thái trước đó.

#### Nên dùng

```bash
git restore README.md
```

#### Không nên dùng

```bash
git reset --hard
```

**Lý do**

`git restore` chỉ tác động đến tệp được chỉ định.

`git reset --hard` sẽ ảnh hưởng đến toàn bộ Repository và có thể làm mất các thay đổi khác.

---

## Reset hay Revert?

### Tình huống

Muốn hoàn tác một Commit.

#### Commit chưa Push

```bash
git reset --soft HEAD~1
```

hoặc

```bash
git reset HEAD~1
```

#### Commit đã Push

```bash
git revert <commit>
```

**Nguyên tắc**

> **Commit đã chia sẻ → Revert**
>
> **Commit chưa chia sẻ → Reset**

---

## Fetch hay Pull?

### Muốn xem thay đổi trước

```bash
git fetch
```

Sau đó:

```bash
git log
git diff
```

---

### Muốn cập nhật ngay

```bash
git pull
```

hoặc

```bash
git pull --rebase
```

---

## Merge hay Rebase?

### Dùng Merge khi

- Làm việc nhóm.
- Branch đã được chia sẻ.
- Muốn giữ nguyên lịch sử.

```bash
git merge feature/login
```

---

### Dùng Rebase khi

- Đồng bộ Feature Branch.
- Dọn lịch sử Commit.
- Branch chỉ do một người sử dụng.

```bash
git rebase main
```

---

## Stash hay Commit?

### Dùng Stash khi

- Chưa hoàn thành công việc.
- Chỉ muốn lưu tạm.
- Cần chuyển Branch ngay.

```bash
git stash
```

---

### Dùng Commit khi

- Hoàn thành một phần công việc.
- Muốn lưu lịch sử.
- Muốn đồng bộ với Remote.

```bash
git commit -m "Implement login API"
```

---

## Cherry-pick hay Merge?

### Chỉ cần một Commit

```bash
git cherry-pick
```

---

### Cần toàn bộ Feature

```bash
git merge
```

---

## Force Push hay Force-with-lease?

### Không nên

```bash
git push --force
```

---

### Nên

```bash
git push --force-with-lease
```

`--force-with-lease` sẽ kiểm tra xem Remote có thay đổi mới hay không trước khi ghi đè.

Đây là lựa chọn an toàn hơn trong môi trường làm việc nhóm.

---

## Switch hay Checkout?

Git khuyến nghị:

```bash
git switch
```

để chuyển Branch.

Thay vì:

```bash
git checkout
```

Lý do:

- Ý nghĩa rõ ràng hơn.
- Ít gây nhầm lẫn.
- Phù hợp với các phiên bản Git hiện đại.

---

## Add hay Add -p?

### Khi toàn bộ thay đổi đều liên quan

```bash
git add .
```

---

### Khi một tệp chứa nhiều thay đổi

```bash
git add -p
```

Cho phép lựa chọn từng phần để đưa vào Staging.

---

## Quy trình lựa chọn lệnh

```text
Có thay đổi cần xử lý?

        │
        ▼

Muốn hủy thay đổi?

        │
        ├──► git restore

        ▼

Muốn hoàn tác Commit?

        │
        ├──► Đã Push?
        │          │
        │          ├── Có ─► git revert
        │          │
        │          └── Không ─► git reset
        │
        ▼

Muốn đồng bộ?

        │
        ├──► Chỉ tải dữ liệu?
        │          │
        │          └──► git fetch
        │
        └──► Cập nhật luôn?
                   │
                   └──► git pull
```

---

## Quy tắc ghi nhớ

### 1. Không chắc thì đừng dùng `--hard`

Nếu còn nghi ngờ, hãy sử dụng:

- `git restore`
- `git reset --soft`
- `git stash`

---

### 2. Không Rewrite lịch sử đã chia sẻ

Không nên:

- Rebase Branch đã Push.
- Force Push lên Branch dùng chung.
- Sửa Commit của người khác.

---

### 3. Luôn kiểm tra trước khi Push

Thực hiện:

```bash
git status

git diff --staged

git log --oneline
```

---

### 4. Mỗi Commit chỉ nên có một mục đích

Ví dụ:

- Thêm tính năng.
- Sửa lỗi.
- Cập nhật tài liệu.

Không nên gộp nhiều thay đổi không liên quan vào cùng một Commit.

---

## Tình huống thực tế

### Đang làm dở nhưng Production gặp lỗi

```text
git stash
        │
        ▼
git switch main
        │
        ▼
Sửa lỗi
        │
        ▼
Commit
        │
        ▼
Push
        │
        ▼
git switch feature
        │
        ▼
git stash pop
```

---

### Commit sai sau khi Push

```text
Đã Push?
    │
    ├── Có
    │     │
    │     └──► git revert
    │
    └── Không
          │
          └──► git reset
```

---

### Muốn lấy Hotfix sang Branch Release

```text
main
 │
 ├── Hotfix Commit
 │
 ▼
git cherry-pick
 │
 ▼
release/v1.2
```

---

## Thực hành tốt

- Luôn chọn lệnh có phạm vi tác động nhỏ nhất.
- Không sử dụng `git reset --hard` nếu chưa hiểu rõ hậu quả.
- Ưu tiên `git revert` khi làm việc trên Branch dùng chung.
- Sử dụng `git fetch` trước khi Merge hoặc Rebase.
- Chỉ Force Push khi thực sự cần thiết và ưu tiên `--force-with-lease`.
- Khi phân vân giữa hai lệnh, hãy kiểm tra bằng `git status` và `git log` trước khi thao tác.

---

## Lệnh liên quan

- `git restore`
- `git reset`
- `git revert`
- `git merge`
- `git rebase`
- `git stash`
- `git cherry-pick`
- `git tag`

---

## Bài tiếp theo

→ [12. Best Practices](./12-best-practices.md)
