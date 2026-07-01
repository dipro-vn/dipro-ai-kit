---
description: Chuẩn hóa bug report từ mô tả lỗi của QC — phân loại severity/priority, sinh steps to reproduce rõ ràng, format sẵn sàng paste vào Backlog.
skills:
  - bug_reporter
---

> **Canonical workflow:** `.claude/agents/qc-agent.md`.
> **BẮT BUỘC:** Nạp skill `bug_reporter` tại `.claude/skills/bug_reporter/SKILL.md`.

# /test/generate_bug_report

## Khi nào dùng

- QC vừa tìm được bug, muốn viết report nhanh
- Chuẩn hóa bug report đang viết dở
- Kiểm tra severity/priority trước khi submit Backlog
- **Không dùng** để sinh test cases

## Input

| Input | Bắt buộc | Mô tả |
|---|---|---|
| Mô tả bug | ✅ | Ngôn ngữ tự nhiên — viết sơ cũng được |
| Environment | ⚠️ | Browser / OS / URL / Build / test account |
| Evidence | ⚠️ | Screenshot, video, console log |
| Module / Feature | ⚠️ | Để gán Bug ID đúng |
| Severity đề xuất | ❌ | Nếu không có → agent tự phân loại |

## Bug ID convention

```
BUG_<MODULE>_<NNN>
ví dụ:
BUG_ORDER_001
BUG_REPORT_023
BUG_PAYMENT_007
```

## Output path

`docs/features/<feature>/bug-reports/<BUG_ID>.md`
(hoặc paste thẳng lên Backlog — không bắt buộc lưu file)

## Các bước

### Bước 1 — Thu thập

1. Đọc mô tả
2. Nếu thiếu info quan trọng → hỏi **gộp 1 lần duy nhất**:
   - Environment
   - Steps cụ thể reproduce
   - Expected behavior
3. Đủ → tiến luôn

### Bước 2 — Phân tích + phân loại

Theo skill `bug_reporter`:
- **Severity** (Critical / Major / Minor / Trivial)
- **Priority** đề xuất + giải thích nếu khác user
- **Reproducibility** (Always / Sometimes / Rare / Once)
- Module để sinh Bug ID

### Bước 3 — Steps to Reproduce

1. Precondition đầy đủ (tài khoản, trạng thái ban đầu, URL)
2. Mỗi step = 1 action
3. Test data cụ thể (theo convention của dự án — không PII thật)
4. Step cuối = trigger lỗi
5. Phân biệt **Expected** vs **Actual**

### Bước 4 — Output theo format skill `bug_reporter`

Nếu nhiều bugs cùng lúc → sinh **từng report riêng**, không gộp.

## Quy tắc

- ❌ KHÔNG gộp nhiều bugs vào 1 report
- ❌ KHÔNG tự đoán steps nếu user mô tả chưa rõ → hỏi
- ❌ KHÔNG log token / password / payment data trong steps
- ❌ KHÔNG auto-submit lên Backlog — QC review rồi mới submit
- ✅ Không có evidence → ghi "Chưa có — QC cần bổ sung"
- ✅ Luôn giải thích lý do Severity/Priority nếu có thể tranh luận
