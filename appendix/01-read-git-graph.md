# Read Git Graph

> Học cách đọc Commit Graph như Git.

---

## Tổng quan

Hầu hết các lệnh Git đều làm việc trên Commit Graph.

Ví dụ:

```text
A → B → C
```

hoặc

```text
A → B → C
     \
      D → E
```

Nếu không đọc được Graph, rất khó hiểu:

- Merge
- Rebase
- Cherry-pick
- Reset
- Reflog

---

## Quy ước

Trong toàn bộ Repository này:

```text
↓

Reference

→

Parent Commit
```

Ví dụ:

```text
HEAD
 │
 ▼
main
 │
 ▼
A → B → C
```

---

## Lab 1

Tạo Repository

```bash
git init

echo A > app.txt
git add .
git commit -m A

echo B >> app.txt
git commit -am B

echo C >> app.txt
git commit -am C
```

Quan sát

```bash
git log --graph --decorate --oneline
```

Kết quả

```text
* C
*
* B
*
* A
```

---

## Lab 2

Tạo Branch

```bash
git switch -c feature

echo D >> app.txt
git commit -am D

echo E >> app.txt
git commit -am E

git switch main

echo F >> app.txt
git commit -am F
```

Quan sát

```bash
git log --graph --decorate --all --oneline
```

Bạn sẽ thấy

```text
        D → E
       /
A → B → C → F
```

---

## Lab 3

Merge

```bash
git merge feature
```

Quan sát

```text
        D → E
       /     \
A → B → C → F → M
```

Hãy xác định:

- Merge Base
- Merge Commit
- Parent của M

---

## Lab 4

Rebase

Tạo Repository mới.

Lặp lại Lab 2.

Sau đó:

```bash
git switch feature

git rebase main
```

Quan sát

```text
A → B → C → F → D' → E'
```

Hãy trả lời:

- D biến mất chưa?
- E biến mất chưa?
- Vì sao Hash thay đổi?

---

## Checklist

Bạn nên tự trả lời được:

✓ HEAD đang ở đâu?

✓ Branch nào đang di chuyển?

✓ Commit nào mới được tạo?

✓ Merge Commit có bao nhiêu Parent?

✓ Commit nào là Merge Base?