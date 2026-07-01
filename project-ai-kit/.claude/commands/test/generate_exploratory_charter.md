---
description: Sinh structured exploratory testing charter — mission, scope, time-box, ghi chép kết quả. Biến exploratory testing từ ad-hoc thành có hệ thống.
skills:
  - rbt_manual_testing
---

> **Canonical workflow:** `.claude/agents/qc-agent.md`.
> **BẮT BUỘC:** Nạp skill `rbt_manual_testing`.

# /test/generate_exploratory_charter

## Khi nào dùng

- Tính năng mới chưa có TC, cần khám phá nhanh
- Sau khi chạy hết TC chính thức, tìm thêm bug ngoài script
- Release gấp, không đủ thời gian viết TC chi tiết
- Test theo góc nhìn user thực tế, không giới hạn bởi script
- **Không thay thế** systematic TC cho module quan trọng

## Input

| Input | Bắt buộc | Mô tả |
|---|---|---|
| Module / Tính năng | ✅ | Tên hoặc URL |
| SPEC.md | ⚠️ Nên có | Để hiểu expected behavior |
| Thời gian có sẵn | ⚠️ Nên có | "45 phút", "2 tiếng" |
| Mục tiêu cụ thể | ❌ | "Tập trung security", "Edge cases" |

## Output path

`docs/features/<feature>/test-cases/exploratory_charter_<topic>.md`

## Các bước

### Bước 1 — Xác định Charter Mission

Đề xuất 1-3 charters (mỗi charter = 1 session có trọng tâm):

| Charter | Mission | Time-box | Focus |
|---|---|---|---|
| #1 | Explore Happy Path từ góc user mới | 30' | Onboarding |
| #2 | Tìm boundary + edge cases validation | 45' | Input validation |
| #3 | Behavior khi mất kết nối / lỗi server | 30' | Error handling |

**Chờ user chọn charter** → Bước 2.

### Bước 2 — Sinh Charter chi tiết

```markdown
## Charter #N: <Mission Statement>

**Tester:** _____
**Bắt đầu:** _____
**Time-box:** X phút

### Mission
<Mô tả mục tiêu session>

### Scope — Nên explore
- <Vùng 1>
- <Vùng 2>

### Out of Scope
- <Vùng bỏ qua>

### Heuristics gợi ý
- **CRUD:** Create → Read → Update → Delete, all states
- **Boundaries:** min, max, vượt giới hạn, rỗng
- **Interruptions:** Reload, back, logout giữa chừng
- **Concurrency:** 2 tab cùng thao tác
- **Permissions:** Thử nhiều role/actor của dự án (xem AGENTS.md)

### Ghi chép trong session
| Thời điểm | Hành động | Quan sát | Bug? |

### Kết quả session
- Bugs tìm được: N
- Areas covered: ...
- Areas NOT covered: ...
- Ghi chú cho session tiếp:
```

## Quy tắc

- ❌ KHÔNG vượt time-box — mất focus
- ❌ KHÔNG skip ghi chép — đây là điểm khác biệt với ad-hoc
- ✅ Mỗi bug → dùng `/test/generate_bug_report` ngay
- ✅ Sau session → review "Areas NOT covered" để plan session tiếp
- ✅ 1 charter = 1 tester = 1 session — không gộp nhiều mission
