---
description: Cập nhật bộ test cases hiện có khi SPEC thay đổi — phân tích delta, xác định TC cần sửa/thêm/xóa, output bộ TC cập nhật kèm Change Log.
skills:
  - rbt_manual_testing
---

> **Canonical workflow:** `.claude/agents/qc-agent.md`.
> **BẮT BUỘC:** Nạp skill `rbt_manual_testing`.

# /test/update_testcases_from_requirements

## Khi nào dùng

- SPEC.md vừa được cập nhật (BA thay đổi AC, clarify ambiguity)
- Q&A với user đã trả lời xong, cần reflect vào TC
- Tính năng mở rộng / thu hẹp scope
- **KHÔNG dùng** khi chưa có TC → `/test/generate_manual_testcases_rbt`
- **KHÔNG dùng** khi >50% TC bị ảnh hưởng → chạy lại từ đầu

## Input

| Input | Bắt buộc | Mô tả |
|---|---|---|
| Bộ TC hiện tại | ✅ | Path `tc_*.md` hoặc paste bảng |
| SPEC.md mới hoặc danh sách thay đổi | ✅ | |
| SPEC.md cũ (để diff) | ⚠️ Nên có | |

## Quy trình

### Bước 1 — Đọc TC hiện tại
- Tóm tắt: số TC, modules cover
- **Chờ user xác nhận** trước khi sang Bước 2

### Bước 2 — Phân tích Delta

| Loại | Mô tả |
|---|---|
| Thêm mới | Rule/field/flow mới chưa có TC |
| Sửa đổi | Logic / constraint thay đổi |
| Xóa bỏ | Rule/field không còn áp dụng |
| Làm rõ | Ambiguity được giải đáp → TC cũ có thể sai assumption |

Bảng:
```
| # | Loại | Mô tả thay đổi | TCs bị ảnh hưởng |
```

**Chờ user xác nhận delta** → Bước 3.

### Bước 3 — Phân loại TC

| Trạng thái | Ký hiệu |
|---|---|
| Giữ nguyên | ✅ KEEP |
| Cần cập nhật | ✏️ UPDATE |
| Cần xóa | 🗑️ REMOVE |
| TC mới | ➕ NEW |

### Bước 4 — Thực hiện cập nhật
- UPDATE: sửa Steps/Expected/Test Data
- NEW: sinh theo `rbt_manual_testing` (Field-Level Validation nếu có field mới)
- REMOVE: đánh dấu xóa, liệt kê Change Log
- KEEP: copy nguyên

### Bước 5 — Output

```markdown
## Change Log
- ✏️ Updated N TCs: TC_003, TC_004 — Max length 50 → 100
- ➕ Added M TCs: TC_031, TC_032 — Email unique validation
- 🗑️ Removed K TCs: TC_012 — Field "Mã nội bộ" bỏ
- ✅ Kept X TCs không đổi
```

Lưu vào cùng path BMAD của TC cũ.

## Quy tắc

- ❌ KHÔNG xóa TC mà không liệt kê Change Log
- ❌ KHÔNG tự đoán thay đổi — chỉ phân tích những gì user cung cấp
- ✅ Test data update theo rule mới nếu constraint thay đổi
