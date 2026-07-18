# Release Interview Cases

## Case 01 - Tạo Release Tag

| Metadata | Value |
|---|---|
| Difficulty | 2/5 |
| Frequency | 4/5 |
| Mastery | 3/5 |
| Expected Level | Mid |
| Topics | annotated tag, push tag |

### Scenario

Bạn cần đánh dấu Release `v2.4.0` có metadata và thông điệp rõ ràng.

### Recommended Answer

```bash
git tag -a v2.4.0 -m "release: v2.4.0"
git push origin v2.4.0
```

Ưu tiên Annotated Tag cho Release chính thức.

### Interviewer Notes

Hỏi khác biệt giữa Lightweight và Annotated Tag.

### Evaluation Rubric

- Junior: biết Tag.
- Mid: chọn Annotated Tag.
- Senior: nói về signed tag khi chuỗi cung ứng yêu cầu.

---

## Case 02 - Tag trỏ nhầm Commit

| Metadata | Value |
|---|---|
| Difficulty | 3/5 |
| Frequency | 2/5 |
| Mastery | 4/5 |
| Expected Level | Senior |
| Topics | tag immutability convention, remote coordination |

### Scenario

Tag Release đã Push nhưng trỏ nhầm Commit.

### Possible Solutions

- Xóa và tạo lại cùng tên Tag.
- Tạo Tag mới như `v2.4.1` hoặc `v2.4.0-fixed`.

### Recommended Answer

Nếu Tag đã được công bố hoặc artifact đã phát hành, ưu tiên tạo phiên bản mới để tránh thay đổi ý nghĩa của Tag đã phân phối. Chỉ sửa cùng tên khi quy trình nội bộ cho phép và mọi bên được phối hợp.

### Interviewer Notes

Đánh giá hiểu biết về tính bất biến theo convention, dù Git kỹ thuật cho phép di chuyển Tag.

### Evaluation Rubric

- Mid: biết xóa và push lại Tag.
- Senior: không mặc định Rewrite Release marker.
- Staff: liên hệ artifact registry, cache và reproducibility.

---

## Case 03 - Hotfix trên phiên bản cũ

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 3/5 |
| Mastery | 5/5 |
| Expected Level | Senior |
| Topics | maintenance branch, cherry-pick, tag |

### Scenario

Production đang chạy `v1.8`, trong khi `main` đã ở `v2.2`. Cần Hotfix bảo mật cho `v1.8` và sau đó đưa Fix vào `main`.

### Possible Solutions

- Tạo Maintenance Branch từ Tag `v1.8`, Commit Fix, Tag patch release, rồi Cherry-pick hoặc port Fix lên `main`.
- Sửa trên `main` trước rồi Cherry-pick ngược về Maintenance Branch nếu code tương thích.

### Recommended Answer

Tạo Branch từ đúng Release lineage, sửa và Release tại đó. Sau đó port Fix sang các dòng phát triển còn được hỗ trợ. Hướng Cherry-pick phụ thuộc nơi Fix được phát triển an toàn nhất.

### Interviewer Notes

Hỏi cách tránh bỏ sót một dòng phiên bản đang support.

### Evaluation Rubric

- Mid: biết Cherry-pick.
- Senior: hiểu multiple release lines.
- Staff: đề xuất quy trình backport tracking và security disclosure.

---

## Case 04 - Reproducible Build từ Tag

| Metadata | Value |
|---|---|
| Difficulty | 4/5 |
| Frequency | 3/5 |
| Mastery | 5/5 |
| Expected Level | Staff |
| Topics | tag, submodule, generated files, build metadata |

### Scenario

Build lại từ Tag cũ cho ra artifact khác bản đã phát hành.

### Recommended Answer

Không chỉ kiểm tra Git Tag. Xác minh dependency lockfile, Submodule Commit, build toolchain, environment, generated assets và external inputs. Git đảm bảo Snapshot của nội dung được Track, không đảm bảo toàn bộ môi trường Build.

### Interviewer Notes

Đây là câu kiểm tra tư duy hệ thống, không chỉ command.

### Evaluation Rubric

- Senior: kiểm tra Tag và Submodule.
- Staff: phân tích toàn bộ supply chain và reproducibility boundary.
