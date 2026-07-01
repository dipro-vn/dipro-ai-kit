---
description: Sync task files thành Backlog issues theo PM workflow. Dùng: /create-backlog <path/to/feature-folder>
---

Đọc `.claude/agents/pm-agent.md` rồi đóng vai **PM (Project Manager)** để sync task files trong feature folder lên Backlog: **$ARGUMENTS**

Toàn bộ workflow nằm trong **Bước 4 — Sync to Backlog** của `pm-agent.md`:
- 4.1 — 5 câu hỏi bắt buộc (Parent Issue / Category / Milestone / Assignee / URL THAM KHẢO base)
- 4.2 — verify Backlog metadata qua `mcp__backlog__*` (read-only)
- 4.3 — mapping task file → Backlog fields (priority theo phase, summary, estimate)
- 4.4 — description template (Backlog Markdown)
- 4.5 — quy tắc: tạo 1 issue mẫu → confirm với user → batch tạo còn lại
- 4.6 — output format: issue keys theo phase

Tuân thủ đầy đủ ràng buộc của pm-agent: KHÔNG sửa task .md files, KHÔNG tạo PLAN mới (đã có sẵn), KHÔNG đoán giá trị metadata.

Nếu `$ARGUMENTS` rỗng hoặc không trỏ tới feature folder hợp lệ (không có `tasks/task-*.md`), hỏi user trước khi bắt đầu.
