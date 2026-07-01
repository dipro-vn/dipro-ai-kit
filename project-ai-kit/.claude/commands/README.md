# Slash Commands

> **Canonical workflow** nằm trong `.claude/agents/*.md`. Commands là thin entry points — không chứa workflow.

## BMAD Core

| Command | Chức năng | Agent |
|---|---|---|
| `/init-kit` | Setup kit cho dự án mới (chạy 1 lần) | `init-agent.md` |
| `/create-spec <feature>` | Tạo SPEC.md | `ba-agent.md` |
| `/create-design <SPEC.md>` | Tạo DESIGN.md per repo | `techlead-design-agent.md` |
| `/create-ui-design <SPEC.md>` | Tạo Figma screens + URL vào SPEC.md ## Screens | `designer-agent.md` |
| `/create-tasks <feature/>` | Phân rã DESIGN → task files | `techlead-tasks-agent.md` |
| `/create-plan <feature/>` | Tạo PLAN.md | `pm-agent.md` |
| `/create-backlog <feature/>` | Sync task files → Backlog issues | `pm-agent.md` |
| `/review-code [path]` | Review code trên branch | repo-specific |
| `/generate-api <module>` | Scaffold NestJS module | `backend-agent` |
| `/create-component <Name> [variant]` | Scaffold React component | `frontend-agent` |

## QC Automation Testing

| Command | Chức năng | Agent |
|---|---|---|
| `/qc-automation <feature-path> <figma-url> <target-app> <website-url>` | Sinh Playwright `.spec.ts` + chạy E2E test + xuất `execution-report.md` | `qc-automation-agent.md` |

> Prefer trigger tự nhiên: **"Hãy là QC Automation, test feature: `<path>`, Figma: `<url>`, app: `<app>`, website: `<url>`"**

## QC Manual Testing (`/test/*`)

> Canonical workflow: `qc-agent.md`

| Command | Chức năng | Skill |
|---|---|---|
| `/test/generate_manual_testcases_rbt` | Sinh TC theo FULL RBT 6 bước | `rbt_manual_testing` (FULL) |
| `/test/generate_testcases_from_requirements` | Sinh TC nhanh (QUICK mode) | `rbt_manual_testing` (QUICK) |
| `/test/update_testcases_from_requirements` | Delta-update TC khi SPEC thay đổi | `rbt_manual_testing` |
| `/test/generate_cross_module_test_plan` | Ma trận Pairwise đa module | `requirements_analyzer` |
| `/test/generate_regression_suite` | Chọn TC chạy lại sau code change | `rbt_manual_testing` |
| `/test/generate_test_execution_checklist` | Checklist ưu tiên trước release | `rbt_manual_testing` |
| `/test/generate_exploratory_charter` | Structured exploratory testing | `rbt_manual_testing` |
| `/test/generate_qc_onboarding_report` | Coverage map + task list QC mới | `rbt_manual_testing` + `requirements_analyzer` |
| `/test/generate_test_data` | Test data positive/negative/boundary/edge | — |
| `/test/generate_bug_report` | Chuẩn hóa bug report cho Backlog | `bug_reporter` |
| `/test/export_to_drive` | Export bảng markdown → Google Sheet | — |

> **thin entry** = command chỉ load agent, không chứa workflow. **standalone** = command có workflow riêng.
