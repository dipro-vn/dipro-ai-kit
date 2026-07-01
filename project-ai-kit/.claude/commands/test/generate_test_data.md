---
description: Sinh test data có cấu trúc (positive/negative/boundary/edge) cho UI form, API payload, data-driven test. Output JSON/CSV/Markdown/TypeScript.
---

> **Canonical workflow:** `.claude/agents/qc-agent.md`.

# /test/generate_test_data

## Input

| Input | Bắt buộc | Mô tả |
|---|---|---|
| Feature / Module | ✅ | "Form Order Create", "API POST /orders" |
| Fields cần data | ⚠️ Nên có | Danh sách + constraints |
| URL / Swagger spec | ❌ | Để inspect validation rules chính xác |
| TC đã có | ❌ | Để sinh data match từng TC |
| Output format | ❌ | `json` (mặc định), `csv`, `markdown`, `code` (TS/Java/Python) |
| Ngôn ngữ | ❌ | Tiếng Việt / English |

## Test data conventions

| Loại field | Test data pattern |
|---|---|
| Email | `qc_<module>_<ts>@example.test` |
| Phone | Dùng test prefix theo locale dự án (xem AGENTS.md) |
| Company code | `QC_COMP_<ts>` |
| Order code | `ORD_QC_<YYYYMMDD>_<NNN>` |
| Payment card | Card sandbox của payment gateway đang dùng (xem docs/integrations/<payment-gateway>.md) |
| User ID | `qc_user_<role>_<ts>` |

> **NGHIÊM CẤM:** dùng email/SĐT/CCCD thật, dùng card thật.

## Output path

`docs/features/<feature>/test-cases/test_data_<module>.<format>`

## Các bước

### Bước 1 — Phân tích Fields + Constraints

| Nguồn | Cách lấy | Ưu tiên |
|---|---|---|
| User cung cấp | Đọc input | ⭐⭐⭐ |
| DOM thực tế | browser_navigate + snapshot | ⭐⭐⭐ |
| Swagger/OpenAPI | Parse schema | ⭐⭐⭐ |
| TC đã có | Đọc steps | ⭐⭐ |
| Đoán từ tên | Kinh nghiệm domain | ⭐ |

Per field: tên, type, required, validation, default, enum, dependency.

**CHECKPOINT** — show bảng Fields, chờ user xác nhận.

### Bước 2 — Sinh data theo 4 categories

#### 2A. Positive (Happy)
Unique + traceable format: `<prefix>_<testName>_<timestamp>`

#### 2B. Negative

| Loại | Ví dụ |
|---|---|
| Missing required | `email: ""` |
| Invalid format | `email: "not-email"` |
| Invalid type | `price: "abc"` |
| Duplicate | `email: existing@test.com` |
| Invalid chars | `name: "<script>alert(1)</script>"` |
| Wrong relationship | `password_confirm ≠ password` |

#### 2C. Boundary

| Loại | Ví dụ (min=6, max=20) |
|---|---|
| Min | `"abcdef"` |
| Min - 1 | `"abcde"` |
| Max | `"a" × 20` |
| Max + 1 | `"a" × 21` |
| Empty | `""` |
| Zero / Negative (number) | `0`, `-1` |

#### 2D. Edge

| Loại | Ví dụ |
|---|---|
| Unicode | `"Nguyễn Văn 🎉"` |
| Very long | `"a" × 10000` |
| Whitespace | `"  email@test.com  "` |
| SQL injection | `"'; DROP TABLE users; --"` |
| HTML | `"<b>bold</b><img src=x onerror=alert(1)>"` |
| Null | `null` |
| Special numbers | `0.1+0.2`, `MAX_SAFE_INTEGER`, `NaN` |
| Date edge | `2024-02-29`, `2024-12-31`, `1970-01-01` |

### Bước 3 — Đóng gói

#### JSON (mặc định)

```json
{
  "module": "Order Create",
  "totalDataSets": 15,
  "positive": [{"id": "POS_01", "description": "...", "data": {...}, "expectedResult": "201 Created"}],
  "negative": [{"id": "NEG_01", "data": {...}, "targetField": "email", "negativeType": "missing_required", "expectedResult": "422: Email required"}],
  "boundary": [...],
  "edgeCases": [...]
}
```

#### Markdown

```markdown
| ID | Category | Description | email | password | Expected |
| POS_01 | Positive | Order thành công | qc_order_<ts>@example.test | Test@12345 | 201 Created |
```

#### Code (TS)

```typescript
export const orderData = {
  positive: { email: `qc_order_${Date.now()}@example.test`, ... },
  negative: { emptyEmail: {...}, ... },
};
```

## Data Rules (BẮT BUỘC)

| # | Rule |
|---|---|
| 1 | Unique — timestamp/random |
| 2 | Traceable — prefix `qc_<module>_<testName>` |
| 3 | **No real PII** — không email/SĐT/CCCD/card thật |
| 4 | Respect constraints validation |
| 5 | Mỗi data set có `expectedResult` |
| 6 | Deterministic seed nếu cần reproducible |

## NGHIÊM CẤM

| ❌ | ✅ |
|---|---|
| Placeholder ("email hợp lệ") | Giá trị cụ thể |
| Hardcode trùng lặp | Random + prefix + timestamp |
| PII thật | `qc_*@example.test` |
| Chỉ positive | Đủ 4 categories |
| Thiếu expectedResult | Bắt buộc per data set |
| Đoán validation không kiểm tra | Inspect DOM/Spec hoặc hỏi |
| Đọc `.env` lấy creds | `[FROM_ENV]` placeholder |
