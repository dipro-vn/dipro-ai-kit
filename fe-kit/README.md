# Adapter: Claude (Claude Code / Cowork / claude.ai)

## 1. Installation

macOS/WSL

```python
curl -fsSL https://claude.ai/install.sh | bash
```

Windows PowerShell

```python
irm https://claude.ai/install.ps1 | iex
```

## 2. Tích hợp fe-kit vào dự án

### Bước 1: Copy thư mục `.claude/` vào root dự án

```bash
# Từ thư mục fe-kit, copy toàn bộ .claude/ vào dự án của bạn
cp -r /path/to/dipro-ai-kit/fe-kit/.claude /path/to/your-project/
```

Hoặc nếu dự án đang có sẵn `.claude/`, merge thủ công từng thư mục con:

```bash
cp -r fe-kit/.claude/agents     your-project/.claude/
cp -r fe-kit/.claude/commands   your-project/.claude/
cp -r fe-kit/.claude/skills     your-project/.claude/
cp    fe-kit/.claude/settings.local.json  your-project/.claude/
```

Sau khi copy, cấu trúc dự án sẽ có dạng:

```
your-project/
├── .claude/
│   ├── agents/          # 5 agent: analyst, architect, developer, tester, reviewer
│   ├── commands/        # 5 workflow: new-feature, bug-fix, code-review, refactoring, test-generation
│   ├── skills/          # ~92 skill (react-*, nextjs-*, typescript-*, mui-*, tailwind-*, ...)
│   └── settings.local.json
├── src/
├── package.json
└── CLAUDE.md            # ← cần tạo ở bước 2
```

### Bước 2: Tạo `CLAUDE.md` cho dự án

Mở Claude Code trong thư mục dự án và chạy prompt sau (thay nội dung trong `[...]`):

```
<role>
Bạn là Tech Lead đang onboard một kỹ sư mới vào [Tên dự án].
</role>

<context>
[Mô tả dự án: làm gì, core logic, tech stack chính, các điểm đặc thù]
</context>

<task>
Tạo file CLAUDE.md ở thư mục gốc.
Áp dụng progressive disclosure bằng cách tạo các file tài liệu riêng trong thư mục `.claude/docs/`.
</task>

<requirements>
- CLAUDE.md tối đa 150 dòng, chỉ tập trung thông tin áp dụng chung cho toàn dự án.
- Dùng Progressive Disclosure: CLAUDE.md là file index, tách chi tiết kỹ thuật ra `.claude/docs/`.
- Viết cho agent/developer chưa từng thấy codebase này.
- Mọi mục phải actionable, không trang trí.
</requirements>

Bao gồm các mục sau trong CLAUDE.md theo đúng thứ tự:
1. Project Overview (2-3 câu)
2. Tech Stack (kèm version)
3. Dev Commands (install, dev, build, test)
4. Core Logic Summary
5. Key Constraints
6. Additional Documentation (link tới .claude/docs/)
```

### Bước 3: Kiểm tra kit hoạt động

Trong Claude Code tại thư mục dự án:

```
/new-feature Thêm trang profile: avatar upload, cập nhật họ tên/email.
```

Nếu Claude phân tích yêu cầu → đề xuất plan → chờ xác nhận → implement, tức là kit đã hoạt động đúng.

### Ví dụ thực tế

Xem [`sample-web-app`](../sample-web-app/) để tham khảo một dự án Next.js đã tích hợp fe-kit hoàn chỉnh, bao gồm `CLAUDE.md` và cấu trúc `.claude/docs/` cụ thể.

---

## 3. Cài đặt FIGMA MCP: https://github.com/vkhanhqui/figma-mcp-go

## 4. Đối ứng task với 1 command

Trong thư mục dự án đã có `CLAUDE.md`, gọi command tương ứng với task:

| Task        | Command            | Trigger khi                                  |
| ----------- | ------------------ | -------------------------------------------- |
| Feature mới | `/new-feature`     | Có story/issue cần build từ đầu              |
| Bug fix     | `/bug-fix`         | Có lỗi cần sửa                               |
| Code review | `/code-review`     | Có PR/changeset cần review trước merge       |
| Refactoring | `/refactoring`     | Cải cấu trúc code, không đổi hành vi         |
| Sinh test   | `/test-generation` | Cần bổ sung/tạo test cho feature hoặc module |

**Ví dụ:**

```
/new-feature Thêm trang profile: avatar upload, cập nhật họ tên/email, lưu qua Server Action.
```

```
/bug-fix Nút submit form checkout bị disabled sau lần submit đầu, không recover khi lỗi network.
```

```
/code-review
```

Claude sẽ tự điều phối toàn bộ pipeline: phân tích yêu cầu → thiết kế → implement → test → review. Hỏi lại nếu còn thông tin chưa đủ, đề xuất plan và chờ xác nhận trước khi viết code.

**Cấu trúc kit:**

```
.claude/
├── agents/          # 5 agent: analyst, architect, developer, tester, reviewer
├── commands/        # 5 workflow: new-feature, bug-fix, code-review, refactoring, test-generation
├── skills/          # ~90 skill theo prefix: react-*, nextjs-*, typescript-*, form-*, react-query-*, mui-*, tailwind-*, security-*, performance-*, testing-jest-*, ...
└── settings.local.json  # Git & security guardrails
```
