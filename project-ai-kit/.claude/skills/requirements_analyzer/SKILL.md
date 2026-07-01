---
name: requirements_analyzer
description: Skill phân tích requirement document có sẵn (Google Drive, Docs, Figma, .md, .doc, Backlog ticket) — hướng dẫn cách đọc, extract AC, và phát hiện ambiguities.
---

# Requirements Analyzer

## Mục tiêu

Hướng dẫn AI cách đọc và phân tích **requirement document có sẵn** (Google Drive, Docs, Figma, .md, .doc, Backlog ticket) — không phải tự sinh requirements từ UI.

---

## Nguyên tắc cốt lõi

- **Đọc kỹ trước khi phân tích** — đọc toàn bộ document, kể cả comments và attachments
- **Không tự đoán business logic** — những gì không được ghi rõ trong document → đưa vào Ambiguities
- **AC do PM tạo** — QC đọc, confirm và map AC vào requirements, không tự sinh AC mới
- **Ưu tiên độ chính xác** — thà hỏi clarify hơn là giả định sai

---

## Kỹ thuật phân tích

### 1. Đọc hiểu scope

- Xác định rõ: tính năng này **thêm mới**, **chỉnh sửa**, hay **xóa** behavior hiện tại?
- Các module/page nào bị ảnh hưởng — trực tiếp và gián tiếp?
- Ai là actor? (user thường, admin, hệ thống tự động?)

### 2. Map AC vào Requirements

Với mỗi REQ trong document:
- Tìm AC tương ứng mà PM đã viết
- Kiểm tra AC đã cover: happy path, negative, edge cases chưa?
- REQ nào thiếu AC → đánh dấu là Ambiguity, không tự bổ sung

### 3. Phát hiện Ambiguities — dấu hiệu cần tìm

| Dấu hiệu | Ví dụ |
|----------|-------|
| Từ khóa mơ hồ | "where applicable", "as needed", "tương tự như", "v.v." |
| Validation thiếu | Không ghi min/max, format, required/optional |
| Behavior edge case chưa mô tả | Mất mạng, dữ liệu trống, concurrent access |
| Inconsistency với mockup | Tên field, format, layout khác nhau |
| Threshold chưa định nghĩa | "tiếp cận deadline" — bao nhiêu ngày? |
| Conflict với requirement cũ | REQ mới mâu thuẫn với behavior hiện tại |

### 4. Phân biệt Business Rules vs UI Rules

- **Business rule:** "Tài khoản bị khóa sau 5 lần sai" → test kỹ, ảnh hưởng nghiệp vụ
- **UI rule:** "Nút Submit disabled khi form chưa đủ dữ liệu" → test validation behavior

### 5. Đọc dependencies

- Tài liệu phụ thuộc → đọc và tóm tắt
- AC từ tài liệu phụ thuộc có thể ảnh hưởng scope của tài liệu chính
- Ghi rõ rule nào đến từ tài liệu nào

---

## Những gì KHÔNG làm

- ❌ Tự sinh AC khi PM chưa viết → đưa vào Ambiguities
- ❌ Tự đoán business logic khi document không nói rõ
- ❌ Bỏ qua comments hoặc notes đính kèm trong document
- ❌ Giả định inconsistency giữa document và mockup là lỗi typo — ghi rõ để confirm
