# Git Handbook

> Giải thích cách Git hoạt động bên trong thay vì chỉ hướng dẫn sử dụng các lệnh.

---

## Giới thiệu

Sau khi hoàn thành phần **Cheat Sheet**, bạn đã biết cách sử dụng hầu hết các lệnh Git trong công việc hằng ngày.

Tuy nhiên, để sử dụng Git thành thạo, bạn cần hiểu:

- Git lưu dữ liệu như thế nào?
- Branch thực chất là gì?
- HEAD trỏ tới đâu?
- Vì sao Merge tạo Conflict?
- Rebase làm thay đổi lịch sử như thế nào?
- Vì sao `git reflog` có thể khôi phục Commit đã mất?

Đó chính là mục tiêu của Handbook.

Khác với Cheat Sheet, Handbook không tập trung vào cú pháp lệnh mà giải thích bản chất hoạt động của Git thông qua sơ đồ, ví dụ và mô hình tư duy.

---

## Đối tượng

Handbook phù hợp với:

- Lập trình viên đã biết sử dụng Git cơ bản.
- Người muốn hiểu Git thay vì chỉ ghi nhớ câu lệnh.
- Người chuẩn bị phỏng vấn về Git.
- Người muốn xử lý các tình huống Git phức tạp.

---

## Nội dung

| Chương | Nội dung |
|---------|----------|
| 01 | Git Mental Model |
| 02 | Git Object Database |
| 03 | Working Tree, Index & HEAD |
| 04 | Commit History & References |
| 05 | Branch & Tag |
| 06 | Remote & Tracking Branch |
| 07 | Merge, Rebase & Conflict |
| 08 | History Rewrite & Recovery |

---

## Cách đọc

Nên đọc theo thứ tự.

Mỗi chương đều xây dựng trên kiến thức của chương trước.

Nếu bỏ qua các chương đầu, các nội dung phía sau sẽ khó hiểu hơn.

---

## Sau Handbook

Sau khi hoàn thành Handbook, bạn sẽ hiểu:

- Git lưu dữ liệu như thế nào.
- Vì sao Git hoạt động nhanh.
- Cách Git quản lý lịch sử.
- Cơ chế Merge và Rebase.
- Cách khôi phục dữ liệu khi xảy ra sự cố.
- Vì sao mỗi lệnh Git hoạt động theo cách của nó.

Khi đó, bạn không chỉ **biết sử dụng Git**, mà còn **hiểu cách Git vận hành**.