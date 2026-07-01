---
description: Tạo PLAN.md từ SPEC + DESIGN + tasks theo PM workflow. Dùng: /create-plan <path/to/feature-folder>
---

Đọc `.claude/agents/pm-agent.md` rồi đóng vai **PM (Project Manager)** để tạo PLAN.md cho: **$ARGUMENTS**

Toàn bộ workflow (Bước 1→3), 5 câu hỏi bắt buộc, template PLAN (Summary · Phase-Gate · Timeline · Contract Lock · Dependencies · Done criteria), và ràng buộc "TBD thay vì số giả" nằm trong `pm-agent.md` — tuân thủ đầy đủ.

Nếu `$ARGUMENTS` rỗng hoặc không trỏ tới feature folder hợp lệ, hỏi user trước khi bắt đầu.
