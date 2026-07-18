# Merge, Rebase & Conflict

> Hiểu bản chất của Merge, Rebase và nguyên nhân gây ra Merge Conflict.

---

## Tổng quan

Merge và Rebase là hai kỹ thuật được sử dụng để kết hợp lịch sử phát triển.

Chúng có cùng mục tiêu:

> Đưa thay đổi từ một nhánh vào một nhánh khác.

Nhưng cách thực hiện hoàn toàn khác nhau.

Nếu hiểu rõ chương này, bạn sẽ trả lời được:

- Merge thực sự làm gì?
- Rebase thực sự làm gì?
- Merge Base là gì?
- Vì sao xảy ra Merge Conflict?
- Khi nào nên Merge, khi nào nên Rebase?

---

## Mental Model

Có hai cách để đưa hai tuyến đường gặp nhau.

### Merge

Xây thêm một nút giao.

```text
A → B → C
         \
          D → E
           \
            M
```

### Rebase

Di chuyển cả tuyến đường sang vị trí mới.

```text
A → B → C → D' → E'
```

---

## Git Internals Diagram

### Merge

```text
main
 │
 ▼
A → B → C
     \
      D → E
           \
            M
```

### Rebase

```text
main
 │
 ▼
A → B → C → D' → E'
```

Trong Rebase:

- D' và E' là Commit mới.
- D và E vẫn tồn tại trong Object Database cho đến khi được dọn dẹp (garbage collection).

---

## Merge là gì?

Merge tạo **một Commit mới** để kết nối hai nhánh.

Ví dụ:

```text
main

A → B → C

feature

      D → E
```

Thực hiện:

```bash
git merge feature
```

Git tạo:

```text
A → B → C
     \     \
      D → E → M
```

Trong đó:

- `M` là Merge Commit.
- `M` có **hai Parent**.

---

## Merge Commit

Commit thông thường:

```text
Commit

Parent = 1
```

Merge Commit:

```text
Commit

Parent 1 = main

Parent 2 = feature
```

Đây là lý do Git vẫn biết hai lịch sử đã hội tụ.

---

## Merge Base

Merge Base là Commit chung gần nhất giữa hai nhánh.

Ví dụ:

```text
A → B → C
     \
      D → E
```

Merge Base là:

```text
B
```

Git luôn so sánh:

- Merge Base
- Branch A
- Branch B

Đây gọi là **Three-way Merge**.

---

## Three-way Merge

Git không chỉ so sánh hai Branch.

Git so sánh ba Snapshot:

```text
          Merge Base
               │
        ┌──────┴──────┐
        ▼             ▼
    main          feature
```

Từ đó Git xác định:

- Dòng nào chỉ thay đổi ở một phía.
- Dòng nào thay đổi ở cả hai phía.
- Dòng nào tạo Conflict.

---

## Merge Conflict

Conflict xảy ra khi:

Hai Branch cùng sửa **cùng một phần** của cùng một tệp kể từ Merge Base.

Ví dụ:

Merge Base

```text
Hello
```

main

```text
Hello World
```

feature

```text
Hello Git
```

Git không biết giữ phiên bản nào.

Kết quả:

```text
<<<<<<< HEAD
Hello World
=======
Hello Git
>>>>>>> feature
```

---

## Inside Git - Merge

Giả sử:

```bash
git merge feature
```

Git thực hiện:

1. Tìm Merge Base.
2. So sánh ba Snapshot.
3. Tự động hợp nhất nếu có thể.
4. Nếu không thể, đánh dấu Conflict.
5. Sau khi Conflict được giải quyết:
   - Tạo Merge Commit.
   - Cập nhật Branch hiện tại.

---

## Rebase là gì?

Rebase **không kết nối lịch sử**.

Rebase:

- Lấy từng Commit.
- Phát lại (Replay) trên nền mới.

Ví dụ:

Ban đầu:

```text
A → B → C
     \
      D → E
```

Thực hiện:

```bash
git rebase main
```

Kết quả:

```text
A → B → C → D' → E'
```

D và E không bị sửa.

Git tạo:

- D'
- E'

Đó là Commit mới.

---

## Inside Git - Rebase

Git thực hiện:

1. Tìm Merge Base.
2. Xác định các Commit cần phát lại.
3. Tạo Commit mới theo cùng thứ tự.
4. Cập nhật Branch sang Commit cuối cùng.

Không có Commit nào được chỉnh sửa.

Git luôn tạo Commit mới.

---

## Merge vs Rebase

| Merge | Rebase |
|--------|---------|
| Tạo Merge Commit | Tạo Commit mới |
| Giữ nguyên lịch sử | Viết lại lịch sử |
| Thích hợp cho Branch dùng chung | Thích hợp cho Branch cá nhân |
| Dễ thấy lịch sử phân nhánh | Lịch sử tuyến tính hơn |
| Không đổi Hash Commit cũ | Hash Commit thay đổi |

---

## Khi nào nên dùng?

### Merge

- Hoàn thành Feature.
- Branch đã chia sẻ.
- Muốn giữ nguyên lịch sử.

---

### Rebase

- Đồng bộ Feature Branch với `main`.
- Dọn dẹp lịch sử trước Pull Request.
- Chưa Push hoặc chưa chia sẻ Branch.

---

## Ví dụ thực tế

Feature Branch:

```text
A → B → C
     \
      D → E
```

Sau nhiều ngày:

```text
A → B → C → F → G
     \
      D → E
```

Nếu Merge:

```text
A → B → C → F → G
     \           \
      D → E ------M
```

Nếu Rebase:

```text
A → B → C → F → G → D' → E'
```

---

## Common Misconceptions

### Rebase nhanh hơn Merge

❌ Sai.

Đây là hai chiến lược quản lý lịch sử.

---

### Merge luôn tạo Conflict

❌ Sai.

Nếu thay đổi độc lập, Git sẽ Merge tự động.

---

### Rebase sửa Commit

❌ Sai.

Rebase tạo Commit mới.

---

### Merge xóa Branch

❌ Sai.

Merge không tự xóa Branch.

---

## 🔬 Experiment 1 - Quan sát Merge Commit

#### Dự đoán

Merge có tạo Commit mới không?

#### Thực hành

```bash
git init

echo "v1" > app.txt
git add .
git commit -m "init"

git switch -c feature

echo "feature" >> app.txt
git commit -am "feature"

git switch main

echo "main" >> app.txt
git commit -am "main"

git merge feature

git log --graph --decorate --oneline
```

#### Quan sát

Bạn sẽ thấy một Merge Commit với hai Parent.

#### 💡 Giải thích

Merge không thay đổi các Commit cũ.

Git tạo thêm một Commit mới để nối hai nhánh.

---

## 🔬 Experiment 2 - Quan sát Rebase

#### Dự đoán

Hash Commit có thay đổi không?

#### Thực hành

```bash
git switch feature

git rebase main

git log --graph --decorate --oneline
```

#### Quan sát

Các Commit trên `feature` có Hash mới.

#### 💡 Giải thích

Rebase phát lại từng Commit trên nền mới.

Mỗi Commit được tạo lại nên Hash cũng thay đổi.

---

## 🔬 Experiment 3 - Tạo Merge Conflict

#### Dự đoán

Git có tự quyết định được không?

#### Thực hành

Trên `main`:

```text
Hello World
```

Trên `feature`:

```text
Hello Git
```

Sau đó:

```bash
git merge feature
```

#### Quan sát

Git dừng Merge và đánh dấu Conflict.

#### 💡 Giải thích

Cả hai Branch đều sửa cùng một vùng của tệp kể từ Merge Base.

Git không thể biết ý định của lập trình viên nên yêu cầu xử lý thủ công.

---

## ⚠️ Pitfall

### Rebase Branch đã chia sẻ

Ví dụ:

```bash
git push origin feature
```

Đồng nghiệp đã Pull.

Bạn tiếp tục:

```bash
git rebase main
git push --force-with-lease
```

Lúc này:

- Hash Commit thay đổi.
- Đồng nghiệp phải đồng bộ lại lịch sử.
- Dễ phát sinh xung đột hoặc nhầm lẫn nếu không phối hợp.

**Khuyến nghị:**

- Chỉ Rebase Branch cá nhân hoặc Branch chưa được chia sẻ.
- Với Branch dùng chung, ưu tiên Merge.

---

## Tóm tắt

- Merge tạo Merge Commit để kết nối lịch sử.
- Merge Commit có hai Parent.
- Merge sử dụng Three-way Merge với Merge Base.
- Rebase phát lại Commit trên nền mới.
- Rebase tạo Commit mới và thay đổi Hash.
- Conflict xảy ra khi hai Branch cùng sửa một vùng nội dung.
- Chọn Merge hay Rebase phụ thuộc vào chiến lược quản lý lịch sử của nhóm, không phải vì một cách "tốt hơn" cách còn lại.

---

## Chương tiếp theo

→ [08. History Rewrite](./08-history-rewrite.md)