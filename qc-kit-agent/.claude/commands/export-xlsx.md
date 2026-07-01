---
description: Export markdown artifact ra file .xlsx — mỗi ## section thành 1 sheet, table format chuẩn, <br> thành newline thật. Dùng sau khi review xong artifact.
---

Export file `.md` ra `.xlsx` ready import Google Sheets.

## Cách dùng

```
/export-xlsx <đường dẫn file .md>
```

Ví dụ:
```
/export-xlsx testing/login/test-cases.md
/export-xlsx testing/login/analysis.md
```

## Thực thi

1. Xác định đường dẫn file `.md` từ input của user (tuyệt đối hoặc tương đối từ root project)
2. Kiểm tra file tồn tại
3. Chạy script:

```bash
/Users/dipro/.venvs/qa-tools/bin/python3 .claude/scripts/md_to_xlsx.py <file_path>
```

4. Báo kết quả: đường dẫn file `.xlsx` vừa tạo (cùng thư mục với file `.md` input)

## Output format

- Mỗi `##` / `###` heading → 1 sheet riêng (tên sheet = tên heading, tối đa 31 ký tự)
- Free text (Summary, ghi chú...) → hiển thị phía trên bảng trong cùng sheet, font italic
- Bảng markdown → Excel table: header bold nền xanh, freeze row 1, wrap text, border
- `<br>` trong cell → newline thật (Alt+Enter style)
- Column width tự động, capped 55 ký tự

## Lưu ý

- File `.xlsx` ghi đè nếu đã tồn tại cùng tên
- Nếu script báo lỗi `ModuleNotFoundError: openpyxl`, chạy: `python3 -m venv /Users/dipro/.venvs/qa-tools && /Users/dipro/.venvs/qa-tools/bin/pip install openpyxl`
