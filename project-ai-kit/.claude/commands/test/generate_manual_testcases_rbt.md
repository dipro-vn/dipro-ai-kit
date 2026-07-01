---
description: Sinh manual test cases chất lượng cao theo quy trình AI-RBT 6 bước (Risk-Based Testing) từ SPEC.md của feature.
skills:
  - rbt_manual_testing
---

> **Canonical workflow:** `.claude/agents/qc-agent.md` — file này chỉ là entry point.
> **BẮT BUỘC:** Nạp và đọc kỹ skill `rbt_manual_testing` (Mode FULL RBT) tại `.claude/skills/rbt_manual_testing/SKILL.md`.

# /test/generate_manual_testcases_rbt — FULL RBT 6 bước

## Input

| Input | Bắt buộc | Mô tả |
|---|---|---|
| SPEC.md của feature | ✅ | `docs/features/<feature>/SPEC.md` |
| Module / scope | ✅ | Module nào trong SPEC sẽ sinh TC lần này |
| Figma / design link | ⚠️ Nên có | Để sinh UI Visual TCs cụ thể |

## Nguyên tắc

- **Mode:** FULL RBT (6 bước **tuần tự**, không gộp)
- **DỪNG checkpoint** tại Bước 2 (Q&A) và Bước 4 (Scenarios review)
- AC trong SPEC.md là nguồn để build Traceability Matrix — 100% AC phải cover
- Tất cả output bằng **Tiếng Việt**
- Output path: `docs/features/<feature>/test-cases/tc_<module>.md`

## 6 bước (theo skill `rbt_manual_testing` Mode FULL RBT)

### Bước 1 — Context & Role-play
1. Đọc SPEC.md + Figma (nếu có)
2. Tóm tắt scope test + xác nhận đã hiểu
3. **Chờ user xác nhận** → Bước 2

### Bước 2 — Analysis & QnA
1. Xác định Happy / Alternate / Exception paths
2. Phát hiện Ambiguities (yêu cầu thiếu/mâu thuẫn/chưa rõ)
3. Nếu có Figma → phát hiện UI ambiguities (Error/Empty/Loading states)
4. Đặt Q&A đánh số (Q1, Q2...) — **DỪNG chờ user trả lời**

### Bước 3 — Decomposition
1. Phân rã feature thành Modules/Sub-modules (theo UI hoặc luồng)
2. Liệt kê Dependencies

### Bước 4 — Traceability
1. Map mỗi AC trong SPEC → Module + REQ-ID
2. Gap Analysis: AC nào chưa có Scenario cover
3. High-Level Scenarios cho từng Module
4. **DỪNG chờ user review scenarios** → Bước 5

### Bước 5 — RBT & TC Generation
1. Risk Level (High/Medium/Low) per Module
2. Sinh TC đầy đủ field: ID, Function, Category, Risk, Scenario, Precondition, Steps, Expected, Test Data (cụ thể!), Priority
3. **Field-Level Validation** — mỗi field input có TC riêng theo bảng trong skill
4. **UI Visual TCs** — 6 states per field, đặt TRƯỚC logic TCs
5. Kỹ thuật: EP, BVA, Decision Table, State Transition tùy bài toán

### Bước 6 — Template Mapping
1. Đóng gói vào bảng Markdown chuẩn
2. Thêm section Traceability AC → TC
3. Lưu vào path BMAD đúng feature
4. Format ID: `<MODULE>_TC_<NNN>` (ví dụ: `ORDER_TC_001`)

## Bảng output

```
| ID | Function Name | Category | Risk Level | Test Scenario | Precondition | Steps | Expected Results | Test Data | Priority |
```

## Traceability section (bắt buộc cuối file)

```markdown
## Traceability — AC → TC
| AC ID | TC IDs cover |
|---|---|
| AC-01 | ORDER_TC_001, ORDER_TC_002 |
```
