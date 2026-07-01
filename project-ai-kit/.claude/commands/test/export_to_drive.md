---
description: Xuất bảng từ file markdown (TC, regression, checklist...) lên Google Sheet qua Google Drive MCP — merge tất cả bảng trong file thành sheets riêng.
---

> **Canonical workflow:** `.claude/agents/qc-agent.md`.

# /test/export_to_drive

Đọc file `.md` được tag, parse tất cả bảng markdown, merge thành CSV và upload lên Google Drive — Drive tự convert thành Google Sheet.

Dùng cho mọi file `.md` có bảng: TC, regression suite, execution checklist, bug list, requirements...

## Cách dùng

```
@<feature>/test-cases/tc_<module>.md /test/export_to_drive
```

## Input

| Input | Bắt buộc | Mô tả |
|---|---|---|
| File `.md` | ✅ | Tag bằng `@<path>` |
| Tên Sheet | ❌ | Mặc định = `<folder-cha>_<filename-no-md>` |
| Folder ID Drive | ❌ | Nếu không có → lưu root Drive |

## Các bước

### Bước 1 — Đọc + xác định prefix
1. Đọc nội dung file
2. Không có file → hỏi lại
3. Không có bảng nào → thông báo + dừng
4. Prefix = `<folder-cha>_<filename-no-md>`
   - Ví dụ: `@features/order/test-cases/tc_order_create.md` → prefix `test-cases_tc_order_create`

### Bước 2 — Parse bảng

1. Tìm block bảng (dòng bắt đầu `|`)
2. Per bảng:
   - Dòng 1 = header
   - Dòng 2 = separator (bỏ)
   - Các dòng còn lại = data
   - Tên bảng = heading `##` / `###` ngay trên block, slugify; fallback `bang-N`
3. Tên Sheet:
   - File 1 bảng → tên = prefix
   - File nhiều bảng → tên = `<prefix>_<tên-bảng>`

### Bước 3 — Convert CSV

- Trim whitespace
- `<br>` → ` | ` (giữ readable trong 1 cell)
- Cell có `,` / `"` / newline → wrap `"..."`
- `"` trong cell → escape `""`

### Bước 4 — Upload Drive

```
title:           <tên Sheet>
textContent:     <CSV>
contentMimeType: text/csv
parentId:        <Folder ID nếu có>
```

Drive tự convert CSV → Google Sheet. Nếu chưa kết nối → tự prompt auth.

### Bước 5 — Báo kết quả

```
✅ Export thành công! (N bảng)
📊 <sheet name 1> — 🔗 <link>  (X rows)
📊 <sheet name 2> — 🔗 <link>  (Y rows)
```

## Quy tắc

- ❌ KHÔNG sửa nội dung khi convert — chỉ format
- ✅ Không có file tag → hỏi lại, không tự đoán
- ✅ Không có bảng → thông báo + dừng
