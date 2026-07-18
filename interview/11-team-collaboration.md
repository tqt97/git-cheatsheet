# Team Collaboration Interview Cases

## Case 01 - Force Push lên Shared Branch

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 3/5 |
| Mastery | 5/5 |
| Expected Level | Senior |
| Topics | force push, branch protection, communication |

### Scenario

Một thành viên Force Push lên Shared Branch làm lịch sử thay đổi và nhiều Pull Request bị ảnh hưởng.

### Recommended Answer

Tạm dừng Push, xác định lịch sử chuẩn, bảo toàn cả hai lineage, phục hồi Branch nếu cần và thông báo nhóm. Sau sự cố, bật Branch Protection và giới hạn Force Push.

### Interviewer Notes

Đánh giá xử lý sự cố và cải tiến quy trình.

### Evaluation Rubric

- Mid: dùng Reflog.
- Senior: phối hợp phục hồi.
- Staff: thêm control phòng ngừa và postmortem.

---

## Case 02 - Commit lớn khó Review

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | atomic commits, interactive rebase, reviewability |

### Scenario

Pull Request có một Commit thay đổi 80 tệp, gồm format, rename và logic.

### Recommended Answer

Không chỉ yêu cầu "squash" hoặc "split" máy móc. Tách mechanical change khỏi behavioral change, giữ Commit build được khi khả thi và giảm noise cho Review. Có thể dùng Interactive Rebase nếu Branch chưa chia sẻ.

### Interviewer Notes

Hỏi ứng viên đề xuất thứ tự Commit nào giúp review tốt nhất.

### Evaluation Rubric

- Mid: yêu cầu tách Commit.
- Senior: tách theo loại thay đổi.
- Staff: tối ưu review, bisect và revert đồng thời.

---

## Case 03 - Hai người cùng làm một Feature Branch

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 4/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | shared branch, merge, rebase policy |

### Scenario

Hai developer cùng Push lên một Feature Branch và thường xuyên Rebase rồi Force Push.

### Recommended Answer

Tránh Rewrite Shared Branch. Dùng Merge hoặc chia thành Branch cá nhân rồi tích hợp qua Pull Request. Nếu buộc Rebase, cần ownership và thời điểm phối hợp rõ ràng.

### Interviewer Notes

Hỏi cách thay đổi workflow để giảm xung đột.

### Evaluation Rubric

- Mid: khuyên Pull thường xuyên.
- Senior: thay đổi Branch ownership.
- Staff: thiết kế integration workflow phù hợp team size.

---

## Case 04 - Quy ước Merge Strategy cho cả tổ chức

| Metadata | Value |
|---|---|
| Difficulty | 5/5 |
| Frequency | 3/5 |
| Mastery | 5/5 |
| Expected Level | Staff |
| Topics | merge strategy, governance, audit, tooling |

### Scenario

Tổ chức muốn chọn một chiến lược mặc định giữa Merge Commit, Squash Merge và Rebase Merge.

### Possible Solutions

- Merge Commit: giữ topology và ranh giới Branch.
- Squash Merge: một Commit cho mỗi Pull Request, dễ Revert ở mức Feature.
- Rebase Merge: lịch sử tuyến tính nhưng giữ nhiều Commit.

### Recommended Answer

Không chọn chỉ dựa trên thẩm mỹ lịch sử. Xác định yêu cầu audit, release, rollback, bisect, contributor skill và tooling. Có thể chọn mặc định nhưng cho phép ngoại lệ có kiểm soát.

### Interviewer Notes

Đánh giá khả năng xây policy thay vì tranh luận sở thích.

### Evaluation Rubric

- Senior: nêu trade-off.
- Staff: liên kết chiến lược với governance và developer experience.
