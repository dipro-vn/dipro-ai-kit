# Skills Registry

> Skills được load on-demand trong từng agent. File này là registry tra cứu — không phải always-loaded.
> Cột "Repo" ghi theo **vai trò** (xem bảng Ecosystem trong `AGENTS.md`), không phải tên repo cụ thể — kit này generic cho mọi dự án dùng cùng stack.

| Skill | Repo | Dùng khi |
|---|---|---|
| `nestjs-best-practices/` | backend | Viết/review NestJS |
| `postgresql/` | backend | Schema, migration, query |
| `redis-development/` | backend | Redis cache pattern |
| `react-expert/` | frontend (all FE repos) | React 19 hooks/component patterns |
| `frontend-review/` | frontend (all FE repos) | Code review React 19 / TanStack v5 / RTK v2 / AntD v6 |
| `flutter-review/` | mobile | Code review Flutter |
| `business-analyst/` | — | Discovery, SPEC template |
| `solution-architect/` | — | Kiến trúc cross-cutting |
| `rbt_manual_testing/` | — | Sinh manual TC (QUICK + FULL RBT 6 bước) — master skill cho `qc-agent` |
| `requirements_analyzer/` | — | Phân tích requirements đa nguồn — chỉ dùng cho `/test/generate_cross_module_test_plan` + `/test/generate_qc_onboarding_report` |
| `bug_reporter/` | — | Chuẩn hóa bug report — severity/priority/repro steps |
| `figma-design/` | frontend (all FE repos) + mobile | Figma MCP tools (read + write), token mapping Figma → design tokens dự án |
| `task-decomposition/` | — | Phân rã task từ DESIGN.md — dùng bởi `techlead-tasks-agent` |
| `project-planning/` | — | Risk, dependency, critical path — dùng bởi `pm-agent` |

> `init-agent` không nằm trong `skills/` (nó là agent, xem `.claude/agents/init-agent.md`) — không liệt kê trong bảng này.
