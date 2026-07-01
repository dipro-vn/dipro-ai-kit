---
description: Sinh manual test cases nhanh từ SPEC.md (QUICK mode — 1 lượt, không qua 6 bước RBT).
skills:
  - rbt_manual_testing
---

> **Canonical workflow:** `.claude/agents/qc-agent.md`.
> **BẮT BUỘC:** Nạp skill `rbt_manual_testing` Mode QUICK.

# /test/generate_testcases_from_requirements — QUICK mode

## Khi nào dùng

- SPEC.md đã rõ ràng, AC đầy đủ, không có ambiguity
- Scope nhỏ — 1 form / 1 endpoint / 1 màn hình
- Cần TC ngay, không cần Traceability Matrix riêng
- Nếu thấy requirements mơ hồ hoặc scope >2 module → **tự động đề xuất** chuyển sang `/test/generate_manual_testcases_rbt`

## Input

| Input | Bắt buộc | Mô tả |
|---|---|---|
| SPEC.md hoặc đoạn requirement | ✅ | Path hoặc paste trực tiếp |
| Module / form name | ✅ | |
| Figma / design link | ⚠️ Nên có | |

## Output path

`docs/features/<feature>/test-cases/tc_<module>.md`

## Quy trình (1 lượt)

1. Đọc requirements + xác định Happy / Negative / Boundary / Edge
2. Áp dụng EP, BVA, Decision Table, State Transition tự động
3. **Field-Level Validation** — liệt kê tất cả input fields, sinh TC validation **riêng từng field** theo bảng trong skill
4. **UI Visual TCs** — screen-level + 6 states per field, đặt TRƯỚC logic TCs
5. Sinh bảng Markdown chuẩn

## Bảng output

```
| ID | Function Name | Category | Risk Level | Test Scenario | Precondition | Steps | Expected Results | Test Data | Priority |
```

## Quy tắc

- ID format: `<MODULE>_TC_<NNN>`
- Test Data **cụ thể**: `qc_order_001@example.test`, không `email hợp lệ`
- Mỗi field 1 TC validation (không gộp)
- Bao phủ Positive + Negative + Boundary + Edge — không chỉ Happy Path
- Test Scenario bắt đầu bằng "Check..." (functional) hoặc prefix `[UI Visual]`
