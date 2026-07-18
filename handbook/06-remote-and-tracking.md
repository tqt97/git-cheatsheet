# Remote & Tracking Branch

> Hiểu cách Git quản lý Remote Repository và cơ chế Tracking Branch.

---

## Tổng quan

Một trong những hiểu lầm phổ biến nhất là:

> Remote Repository là Repository "chính".

Thực tế:

Git **không có khái niệm Repository chính**.

Mỗi Repository đều bình đẳng.

Remote chỉ là:

> Một Repository khác mà Repository hiện tại biết cách kết nối.

Hiểu chương này sẽ giúp bạn nắm được:

- Remote thực chất là gì.
- origin chỉ là một tên mặc định.
- Remote Branch hoạt động như thế nào.
- Tracking Branch là gì.
- Vì sao `git fetch` không làm thay đổi mã nguồn.
- Vì sao `git pull` có thể gây Conflict.

---

## Mental Model

Hãy tưởng tượng mỗi Repository là một "hòn đảo".

```text
Local Repository  ←────────→  Remote Repository
```

Git không đồng bộ tự động.

Mọi thay đổi đều phải được:

- Fetch
- Pull
- Push

---

## Git Internals Diagram

```text
             Local Repository

                 HEAD
                  │
                  ▼
                main
                  │
                  ▼
			A → B → C

                  ▲
                  │
            origin/main

──────────────────────────────────────────────

            Remote Repository

                  main
                   │
                   ▼
	      A → B → C
```

Lưu ý:

`origin/main` **không nằm trên Remote**.

Nó nằm trong Local Repository.

---

## Remote là gì?

Remote chỉ là một địa chỉ Repository.

Ví dụ:

```bash
git remote -v
```

```text
origin  git@github.com:user/project.git
```

Trong đó:

- origin là tên.
- URL là địa chỉ.

Bạn hoàn toàn có thể đặt:

```text
github

production

backup

company
```

---

## origin không có ý nghĩa đặc biệt

Khi Clone:

```bash
git clone https://github.com/user/project.git
```

Git tạo:

```text
origin
```

Đây chỉ là tên mặc định.

Có thể đổi:

```bash
git remote rename origin github
```

Git vẫn hoạt động bình thường.

---

## Remote Branch là gì?

Sau:

```bash
git fetch
```

Git tạo các Reference:

```text
refs/remotes/origin/main

refs/remotes/origin/dev
```

Đây gọi là:

Remote Tracking Branch.

Ví dụ:

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C

▲
│
origin/main
```

---

## Tracking Branch

Branch Local có thể "theo dõi" một Remote Branch.

Ví dụ:

```text
main

↓

origin/main
```

Điều này giúp:

```bash
git pull
```

Git biết cần Pull từ đâu.

```bash
git push
```

Git biết cần Push tới đâu.

---

## Inside Git

Sau:

```bash
git fetch origin
```

Git sẽ:

- Kết nối Remote.
- Tải Object mới.
- Cập nhật `origin/main`.

Git **không**:

- Thay đổi Working Tree.
- Thay đổi Index.
- Thay đổi Branch hiện tại.

---

## Git Internals sau Fetch

Ban đầu:

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C

▲
│
origin/main
```

Remote có Commit mới:

```text
A → B → C → D
```

Sau Fetch:

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C

          ▲
          │
     origin/main
          │
          ▼
          D
```

Lưu ý:

Working Tree vẫn ở Commit C.

---

## Pull thực chất là gì?

`git pull`

=

```text
git fetch

↓

git merge
```

Hoặc nếu cấu hình:

```text
git fetch

↓

git rebase
```

Đó là lý do Pull có thể gây Merge Conflict.

---

## Push thực chất là gì?

```bash
git push
```

Git sẽ:

- Gửi Object còn thiếu.
- Yêu cầu Remote cập nhật Branch.

Ví dụ:

Local

```text
A → B → C → D
```

Remote

```text
A → B → C
```

Sau Push:

Remote:

```text
A → B → C → D
```

---

## Fast-forward

Nếu Remote chỉ thiếu Commit mới:

```text
A → B → C
```

↓

```text
A → B → C → D
```

Git chỉ cần di chuyển Reference.

Đây gọi là:

Fast-forward.

---

## Non Fast-forward

Nếu:

Local

```text
A → B → C → D
```

Remote

```text
A → B → C → E
```

Push sẽ bị từ chối.

Git yêu cầu:

```bash
git fetch

git pull
```

hoặc:

```bash
git pull --rebase
```

---

## Common Misconceptions

### origin là GitHub

❌ Sai.

origin chỉ là tên.

---

### origin/main nằm trên GitHub

❌ Sai.

origin/main nằm trong Local Repository.

---

### Fetch cập nhật mã nguồn

❌ Sai.

Fetch chỉ cập nhật Remote Tracking Branch.

---

### Pull là một lệnh riêng

❌ Sai.

Pull = Fetch + Merge/Rebase.

---

### Push sao chép toàn bộ Repository

❌ Sai.

Git chỉ gửi Object còn thiếu.

---

## 🔬 Experiment 1 - Quan sát origin/main

#### Dự đoán

Sau Fetch:

- main có thay đổi không?
- origin/main có thay đổi không?

#### Thực hành

```bash
git fetch

git branch -av
```

#### Quan sát

`origin/main`

được cập nhật.

`main`

không thay đổi.

#### 💡 Giải thích

Git luôn cập nhật Remote Tracking Branch trước.

Branch Local chỉ thay đổi khi Merge hoặc Rebase.

---

## 🔬 Experiment 2 - Pull = Fetch + Merge

#### Dự đoán

Pull sẽ thực hiện những bước nào?

#### Thực hành

```bash
git fetch

git log --graph --decorate --all

git merge origin/main
```

Sau đó thử:

```bash
git pull
```

So sánh kết quả.

#### 💡 Giải thích

Trong cấu hình mặc định, hai quy trình cho kết quả giống nhau.

Điều này chứng minh:

```text
Pull

↓

Fetch

↓

Merge
```

---

## 🔬 Experiment 3 - Theo dõi Remote Tracking Branch

#### Thực hành

```bash
git branch -vv
```

Quan sát:

```text
main  abc123 [origin/main]
```

#### 💡 Giải thích

Git lưu thông tin Tracking để biết:

- Pull từ đâu.
- Push tới đâu.

---

## ⚠️ Pitfall

### Nhầm lẫn giữa `main` và `origin/main`

Nhiều người nghĩ:

```text
main == origin/main
```

Điều này chỉ đúng khi hai Branch đang đồng bộ.

Sau:

```bash
git fetch
```

rất có thể:

```text
main

↓

Commit C

origin/main

↓

Commit D
```

Nếu không quan sát `git status`, `git log --graph` hoặc `git branch -vv`, bạn rất dễ hiểu nhầm rằng mã nguồn đã được cập nhật.

---

## Tóm tắt

Sau chương này cần ghi nhớ:

- Remote là một Repository khác.
- origin chỉ là tên mặc định.
- `origin/main` là Remote Tracking Branch trong Local Repository.
- Fetch chỉ cập nhật Remote Tracking Branch.
- Pull = Fetch + Merge/Rebase.
- Push gửi các Object còn thiếu và cập nhật Reference trên Remote.
- Local Branch và Remote Tracking Branch là hai Reference khác nhau.

---

## Chương tiếp theo

→ [07. Merge, Rebase & Conflict](./07-merge-rebase-and-conflict.md)