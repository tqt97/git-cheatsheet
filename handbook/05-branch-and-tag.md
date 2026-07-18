# Branch & Tag

> Hiểu bản chất của Branch và Tag thay vì xem chúng là "bản sao" của Repository.

---

## Tổng quan

Branch và Tag là hai khái niệm được sử dụng hằng ngày trong Git.

Tuy nhiên, rất nhiều người vẫn hiểu sai rằng:

- Branch là một bản sao của Repository.
- Tag là một Branch đặc biệt.

Thực tế, cả hai đều chỉ là **Reference**.

Điểm khác biệt nằm ở việc Reference đó **có di chuyển hay không**.

Sau chương này, bạn sẽ hiểu:

- Branch thực chất là gì.
- Tag thực chất là gì.
- Vì sao tạo Branch gần như tức thời.
- Vì sao Tag phù hợp để đánh dấu Release.
- HEAD hoạt động như thế nào khi chuyển Branch hoặc Checkout Tag.

---

## Mental Model

Hãy tưởng tượng Commit giống như các toa tàu.

```text
A ---- B ---- C ---- D
```

Branch chỉ là một tấm biển chỉ vị trí toa cuối.

```text
main
  │
  ▼
A ---- B ---- C ---- D
```

Khi có Commit mới:

```text
main
  │
  ▼
A ---- B ---- C ---- D ---- E
```

Tấm biển được dời sang toa mới.

Các toa cũ không thay đổi.

---

## Git Internals Diagram

```text
                    HEAD
                     │
                     ▼
                  main
                     │
                     ▼
+---------+    +---------+    +---------+    +---------+
|Commit A | -> |Commit B | -> |Commit C | -> |Commit D |
+---------+    +---------+    +---------+    +---------+

                    ▲
                    │
               v1.0.0 (Tag)
```

Trong sơ đồ:

- HEAD → Branch
- Branch → Commit mới nhất
- Tag → Một Commit cố định

---

## Branch là gì?

Branch chỉ là một Reference.

Ví dụ:

```text
main

↓

Commit D
```

Khi tạo Branch:

```bash
git switch -c feature/login
```

Git chỉ tạo thêm một Reference.

```text
                 main
                   │
                   ▼
A ---- B ---- C ---- D
                   ▲
                   │
            feature/login
```

Không có Commit mới.

Không có Blob mới.

Không sao chép Repository.

---

## Branch di chuyển như thế nào?

Giả sử đang ở:

```text
feature/login

↓

Commit D
```

Commit mới:

```bash
git commit
```

Kết quả:

```text
main

↓

Commit D

↓

Commit E

↑

feature/login
```

Branch hiện tại được cập nhật.

Branch khác không thay đổi.

---

## Tag là gì?

Tag cũng là Reference.

Nhưng khác Branch ở một điểm:

Tag **không di chuyển**.

Ví dụ:

```text
main

↓

Commit E
```

Tạo Tag:

```bash
git tag v1.0.0
```

```text
v1.0.0

↓

Commit E
```

Sau này:

```text
main

↓

Commit G
```

Tag vẫn:

```text
v1.0.0

↓

Commit E
```

---

## Branch và Tag khác nhau thế nào?

| Branch | Tag |
|---------|-----|
| Di chuyển sau mỗi Commit | Luôn cố định |
| Dùng để phát triển | Dùng để đánh dấu phiên bản |
| Có thể tiếp tục Commit | Không tiếp tục Commit |

---

## Inside Git

Khi chạy:

```bash
git switch -c feature/payment
```

Git thực hiện:

```text
refs/heads/feature/payment

↓

Commit hiện tại
```

Chỉ tạo một file Reference mới trong:

```text
.git/refs/heads/
```

Không tạo Commit.

Không tạo Tree.

Không tạo Blob.

---

## HEAD và Branch

Thông thường:

```text
HEAD

↓

main

↓

Commit D
```

Sau:

```bash
git switch feature/login
```

```text
HEAD

↓

feature/login

↓

Commit F
```

HEAD luôn theo Branch đang Checkout.

---

## HEAD khi Checkout Tag

Ví dụ:

```bash
git checkout v1.0.0
```

Kết quả:

```text
HEAD

↓

Commit E
```

HEAD không còn trỏ tới Branch.

Git chuyển sang **Detached HEAD**.

---

## Ví dụ thực tế

Repository:

```text
A ---- B ---- C
```

Tạo Branch:

```bash
git switch -c feature/api
```

```text
               main
                 │
                 ▼
A ---- B ---- C
                 ▲
                 │
          feature/api
```

Commit:

```bash
git commit
```

```text
main

↓

C

↓

D

↑

feature/api
```

Chỉ `feature/api` được cập nhật.

---

## Common Misconceptions

### Branch là bản sao của Repository

❌ Sai.

Branch chỉ là một Reference.

---

### Tag là Branch

❌ Sai.

Tag không di chuyển.

---

### Tạo Branch rất tốn thời gian

❌ Sai.

Git chỉ tạo một Reference mới.

---

### Checkout Tag vẫn đang ở Branch

❌ Sai.

Git chuyển sang Detached HEAD.

---

## 🔬 Experiment 1 - Branch không sao chép dữ liệu

#### Dự đoán

Sau khi tạo Branch mới:

- Có Commit mới không?
- Có tệp mới không?

#### Thực hành

```bash
git init

echo hello > app.txt

git add .
git commit -m "init"

git switch -c feature/demo

git log --graph --decorate --oneline
```

#### Quan sát

Bạn sẽ thấy:

- Chỉ có một Commit.
- Hai Branch cùng trỏ tới Commit đó.

#### 💡 Giải thích

Branch chỉ là một Reference.

Git không cần sao chép bất kỳ Object nào trong Object Database.

Đó là lý do tạo Branch gần như tức thời.

---

## 🔬 Experiment 2 - Branch di chuyển

#### Dự đoán

Commit mới sẽ làm Branch nào thay đổi?

#### Thực hành

```bash
echo world >> app.txt

git add .
git commit -m "update"

git log --graph --decorate --oneline
```

#### Quan sát

Chỉ Branch hiện tại được cập nhật.

Branch còn lại vẫn đứng yên.

#### 💡 Giải thích

Git luôn cập nhật Reference của Branch đang Checkout.

Các Branch khác không bị ảnh hưởng.

---

## 🔬 Experiment 3 - Tag không di chuyển

#### Dự đoán

Sau khi tạo Tag rồi Commit tiếp:

Tag có thay đổi không?

#### Thực hành

```bash
git tag v1.0.0

echo "v2" >> app.txt

git add .
git commit -m "v2"

git log --graph --decorate --oneline
```

#### Quan sát

Tag vẫn trỏ tới Commit cũ.

#### 💡 Giải thích

Tag được thiết kế để đánh dấu một thời điểm cố định trong lịch sử.

Git không tự động cập nhật Tag khi có Commit mới.

---

## ⚠️ Pitfall

#### Tưởng rằng Checkout Tag là an toàn để tiếp tục phát triển

Ví dụ:

```bash
git checkout v1.0.0
```

Sau đó:

```bash
git commit
```

Commit mới sẽ không thuộc bất kỳ Branch nào.

Nếu quên tạo Branch mới trước khi rời khỏi trạng thái Detached HEAD, Commit đó rất dễ bị "mất dấu" (dù vẫn còn trong Object Database một thời gian và có thể khôi phục bằng `git reflog`).

Giải pháp:

```bash
git switch -c hotfix/v1.0.1
```

---

## Tóm tắt

Sau chương này cần ghi nhớ:

- Branch chỉ là một Reference.
- Branch không chứa mã nguồn.
- Branch di chuyển sau mỗi Commit.
- Tag cũng là Reference.
- Tag luôn cố định.
- HEAD thường trỏ tới Branch.
- Checkout Tag sẽ đưa Git vào Detached HEAD.
- Tạo Branch không sao chép Repository.

---

## Chương tiếp theo

→ [06. Remote & Tracking Branch](./06-remote-and-tracking.md)