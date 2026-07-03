# STEP2: Mục đích connect các MCP phục vụ quá trình phát triển.

### MCP Figma: Cung cấp cho AI các tài liệu về thiết kế giao diện từ file figma

LINK: https://github.com/glips/figma-context-mcp

- Cách tạo API key: https://www.framelink.ai/docs/quickstart#figma-access-token
- Tạo xong hãy thêm vào FIGMA_API_KEY ở file mobile-ai-kit/.mcp.json

### NotebookLM MCP: Cung cấp cho AI các tài liệu về dự án

LINK: https://github.com/jacob-bd/notebooklm-mcp-cli

1. uv tool install notebooklm-mcp-cli
2. claude mcp add notebooklm-mcp -- notebooklm-mcp # Claude Code
3. nlm login # Login account chưa notes
4. nlm notebook list # Lấy noteId của dự án
