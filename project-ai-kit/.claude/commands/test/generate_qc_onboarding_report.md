---
description: Tổng hợp hiện trạng dự án cho QC mới join giữa chừng — coverage map, open bugs, immediate task list. Giảm ramp-up từ ngày xuống giờ.
skills:
  - rbt_manual_testing
  - requirements_analyzer
---

> **Canonical workflow:** `.claude/agents/qc-agent.md`.
> **BẮT BUỘC:** Nạp 2 skill `requirements_analyzer` + `rbt_manual_testing`.

# /test/generate_qc_onboarding_report

## Khi nào dùng

- QC mới được assign vào dự án đang chạy
- QC cũ quay lại sau thời gian dài
- Lead QC audit coverage trước khi plan sprint mới
- **Không dùng** cho dự án mới chưa có artifact → `/test/generate_manual_testcases_rbt`

## Input

Cung cấp bất cứ gì có sẵn — workflow chấp nhận input thiếu:

| Input | Bắt buộc | Mô tả |
|---|---|---|
| Tên + mô tả ngắn | ✅ | Ví dụ: "<Tên dự án> — mô tả ngắn gọn domain nghiệp vụ" |
| Bộ TC hiện có | ⚠️ | Path `docs/features/*/test-cases/` |
| SPEC.md đã có | ⚠️ | Path `docs/features/*/SPEC.md` |
| Bug đang mở | ⚠️ | Từ Backlog hoặc paste |
| Sprint hiện tại / Roadmap | ❌ | |
| Role QC mới | ❌ | Junior/Senior, manual/automation |

> Thiếu input nào → agent ghi "Không có thông tin — cần hỏi team", không tự đoán.

## Output path

`docs/qc-onboarding/<qc_name>_<date>.md` (hoặc path user chỉ định)

## Các bước

### Bước 1 — Thu thập artifacts
- Đọc input
- Liệt kê có/thiếu:
  ```
  ✅ Có: SPEC Login, TC 45 cases, Bug list 12
  ❌ Thiếu: TC Payment, Roadmap sprint tiếp
  ```
- Xác nhận với user.

### Bước 2 — Project Snapshot (≤1 trang)
- Hệ thống làm gì, đối tượng dùng, stack
- Giai đoạn hiện tại (dev / stabilize / pre-release)
- Modules đã xong vs đang dev vs chưa bắt đầu

### Bước 3 — Coverage Map

```markdown
| Module | SPEC | TC hiện có | Coverage | Chất lượng | Trạng thái |
|---|---|---|---|---|---|
| Login | ✅ | 12 | 🟢 Đủ | Tốt | Stable |
| Payment | ✅ | 3 | 🔴 Thiếu | Chỉ Happy Path | Đang dev |
| Profile | ❌ | 0 | ⚫ Chưa có | — | Chưa bắt đầu |
| Order | ✅ | 8 | 🟡 Một phần | Thiếu edge | Stable |
```

- 🟢 Đủ — Happy + Negative + Boundary
- 🟡 Một phần — thiếu vài loại
- 🔴 Thiếu — chỉ Happy hoặc rất ít TC
- ⚫ Chưa có — 0 TC

### Bước 4 — Open Bugs Summary

```markdown
| Severity | Count | Module |
|---|---|---|
| Critical | 1 | Payment |
| Major | 4 | Auth(2), Order(2) |
| Minor | 7 | Nhiều modules |

### Bugs cần chú ý
- BUG-PAY-003 Critical — Payment double charge — investigating
- BUG-AUTH-007 Major — JWT refresh fail — assigned DEV, ETA sprint này
```

### Bước 5 — Immediate Task List

```markdown
## Tác vụ ưu tiên cho <QC name> — Tuần đầu

### 🔴 Làm ngay (Ngày 1-2)
1. Đọc SPEC Payment — coverage thấp nhất, đang active dev
2. Verify BUG-PAY-003 — reproduce confirm
3. Hỏi Lead QC các câu bên dưới

### 🟡 Trong tuần (Ngày 3-5)
4. Bổ sung TC Payment — `/test/generate_manual_testcases_rbt`
5. Bổ sung Negative/Edge cho Order — coverage 🟡
6. Regression smoke Auth + Profile trước sprint release

### 🟢 Để sau
7. Xây TC Profile (0 TC)
8. Review TC cũ nếu SPEC thay đổi
```

### Bước 6 — Câu hỏi cần hỏi team

```markdown
### Hỏi Lead QC
- Q1: TC Payment bị dở vì lý do gì?
- Q2: Test env riêng hay chung dev?

### Hỏi PM/BA
- Q3: Sprint tiếp release tính năng gì?

### Hỏi DEV
- Q4: BUG-PAY-003 status?
```

## Output

```markdown
# QC Onboarding Report
Ngày: <date> | Tạo cho: <QC name>

## 1. Project Snapshot
## 2. Coverage Map
## 3. Open Bugs Summary
## 4. Immediate Task List
## 5. Câu hỏi cần hỏi team
## 6. Thiếu thông tin cần bổ sung
```

## Quy tắc

- ❌ KHÔNG đoán coverage nếu không có TC/SPEC để so → ghi "Không đủ thông tin"
- ❌ KHÔNG sinh task chung chung ("đọc tài liệu") — phải cụ thể
- ✅ Task list khả thi trong tuần đầu
- ✅ Luôn có "Câu hỏi cần hỏi team"
- ✅ Input ít vẫn sinh được report — chỉ ghi rõ chỗ cần bổ sung
