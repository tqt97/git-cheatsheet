# Git Internals Lab

> Khám phá Git từ bên trong bằng cách quan sát Repository thực tế.

---

## Giới thiệu

Sau khi hoàn thành Handbook, bạn đã hiểu:

- Git lưu Object.
- Branch là Reference.
- HEAD là Reference.
- Commit là Immutable.
- Merge và Rebase hoạt động như thế nào.

Bây giờ là lúc kiểm chứng những kiến thức đó.

Trong Lab này, chúng ta sẽ không chỉ sử dụng Git, mà còn trực tiếp quan sát:

- thư mục `.git`
- Object Database
- Reference
- Hash
- các lệnh Plumbing

Bạn sẽ thấy những gì Git thực sự lưu trữ thay vì chỉ đọc mô tả.

---

## Các bài Lab

| Bài | Nội dung |
|------|----------|
| 01 | Đọc Commit Graph |
| 02 | Khám phá `.git` |
| 03 | Những hiểu lầm phổ biến về Git |
| 04 | Plumbing vs Porcelain |

---

## Điều kiện

Bạn nên hoàn thành:

- Cheat Sheet
- Handbook

trước khi thực hiện Lab này.

---

## Mục tiêu

Sau Appendix, bạn có thể:

- đọc Commit Graph phức tạp
- tự kiểm tra Object Database
- giải thích cách Git lưu dữ liệu
- sử dụng một số Plumbing Command
- debug Repository khi cần