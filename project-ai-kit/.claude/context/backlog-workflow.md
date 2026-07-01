# 📋 BACKLOG GUIDELINE

> **Tài liệu này định nghĩa các quy tắc, cấu trúc và phân quyền khi sử dụng Backlog trong dự án.**
> Mọi thành viên trong dự án cần đọc và tuân thủ hướng dẫn này.
>
> Kit default — chỉ cần dùng nếu dự án track issue trên Backlog (`@nulab/backlog-mcp-server`). Nếu dùng tool khác (Jira, Linear...), thay file này bằng guideline tương ứng.

---

## I. Thông Tin Cơ Bản

### 1. Version


| Version   | Mô tả                          |
| --------- | ------------------------------ |
| `Phase 1` | Giai đoạn phát triển đầu tiên  |
| `Phase 2` | Giai đoạn phát triển tiếp theo |


---

### 2. Milestone


| Milestone      | Mô tả                                      |
| -------------- | ------------------------------------------ |
| `Released xxx` | Bản phát hành chính thức                   |
| `Go-live yyy`  | Thời điểm hệ thống đi vào vận hành thực tế |


---

### 3. Category (Epic)

Category tương ứng với từng **Epic/repo** của dự án — điền theo bảng Ecosystem trong `AGENTS.md`. Mỗi issue phải được gán vào đúng Category.


| Category | Mô tả |
| --- | --- |
| _(vd: `Backend_API`)_ | _(1 dòng mô tả — điền qua `/init-kit` hoặc thủ công theo bảng Ecosystem)_ |
| _(vd: `Admin_Web`)_ | |
| _(vd: `Mobile_App`)_ | |


---

### 4. Issue Type


| Issue Type         | Mô tả                                              | Người tạo          |
| ------------------ | -------------------------------------------------- | ------------------ |
| `User Story`       | Mô tả tính năng từ góc nhìn người dùng             | PM / BrSE / Leader |
| `Task`             | Công việc kỹ thuật cụ thể để hoàn thành Story      | Thành viên dự án   |
| `Bug`               | Lỗi phát sinh trong quá trình phát triển hoặc test | Thành viên dự án   |
| `Internal Request` | Yêu cầu nội bộ trong team                          | Thành viên dự án   |
| `Customer Request` | Yêu cầu trực tiếp từ khách hàng                    | PM / BrSE / Leader |


---

### 5. Status &amp; Workflow

```auto
          ┌─────────────────────────────────────┐
          │                                     │
┌─────────▼──────────┐                         │
│        Open        │                         │
└─────────┬──────────┘                         │
          │                                     │
┌─────────▼──────────┐                   Reopen│
│    In Progress     │                         │
└─────────┬──────────┘                         │
          │                                     │
┌─────────▼──────────┐                         │
│  Request Review    │                         │
└─────────┬──────────┘                         │
          │                                     │
┌─────────▼──────────┐                         │
│     In Review      │                         │
└─────────┬──────────┘                         │
          │                                     │
┌─────────▼──────────┐                         │
│  Testing Request   │◄────────────────────────┘
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│      Resolved      │
└────────┬───────────┘
         │
┌────────▼───────────┐
│       Closed       │
└────────────────────┘

```


| #   | Status            | Mô tả                                       | Chuyển tiếp         |
| --- | ----------------- | ------------------------------------------- | ------------------- |
| 1   | `Open`            | Issue vừa được tạo, chưa có người xử lý     | → In Progress       |
| 2   | `In Progress`     | Đang được phát triển / xử lý                | → Request Review    |
| 3   | `Request Review`  | Yêu cầu review từ Leader / PM               | → In Review         |
| 4   | `In Review`       | Đang trong quá trình review                 | → Testing Request   |
| 5   | `Testing Request` | Yêu cầu QC tiến hành kiểm thử               | → Resolved / Reopen |
| 6   | `Resolved`        | QC đã verify xong, chờ đóng chính thức      | → Closed            |
| 7   | `Reopen`          | Issue bị mở lại do chưa đạt yêu cầu         | → Testing Request   |
| 8   | `Closed`          | Đã hoàn thành toàn bộ, được đóng chính thức | —                   |


---

## II. Thông Tin Bắt Buộc (Required Fields)

> ⚠️ **Mọi issue khi tạo mới PHẢI điền đầy đủ các trường sau. Issue thiếu thông tin sẽ bị trả lại.**


| Field                   | Bắt buộc                | Ghi chú                                                |
| ----------------------- | ------------------------ | ------------------------------------------------------ |
| **Title / Name**        | ✅ Bắt buộc              | Tiêu đề ngắn gọn, rõ ràng, mô tả đúng nội dung issue   |
| **Parent Issue**        | ✅ Bắt buộc (Task / Bug) | Task và Bug phải liên kết với User Story hoặc Epic cha |
| **Estimate Hour**       | ✅ Bắt buộc              | Số giờ ước tính để hoàn thành                          |
| **Actual Hour**         | ✅ Bắt buộc              | Số giờ thực tế đã dành (cập nhật khi Resolved)         |
| **Category**            | ✅ Bắt buộc              | Chọn đúng Epic tương ứng                               |
| **Version / Milestone** | ✅ Bắt buộc              | Gán đúng Phase và Milestone                            |


### Ghi chú về Title / Name

Tiêu đề nên theo format:

```auto
[ROLE] [Category] - [Mô tả ngắn gọn hành động]

```

**ROLE** là vai trò / loại issue, viết tắt theo bảng sau:


| ROLE Tag     |
| ------------ |
| `[BE]`       |
| `[FE]`       |
| `[QC]`       |
| `[DESIGNER]` |
| `[INFRA]`    |


## III. Phân Quyền

### 3.1 Định nghĩa Role


| Role          | Mô tả                                                    |
| ------------- | ---------------------------------------------------------- |
| `PM`          | Project Manager                                          |
| `BrSE`        | Bridge System Engineer (cầu nối kỹ thuật với khách hàng) |
| `Team Leader` | Leader của từng nhóm chức năng                           |
| `Tech Leader` | Technical Leader toàn dự án                              |
| `Developer`   | Lập trình viên (Frontend / Backend / Mobile)             |
| `QC / Tester` | Kiểm thử chất lượng                                      |
| `BA`          | Business Analyst                                         |
| `DESIGNER`    | Designer                                                 |


---

### 3.2 Quyền Tạo Issue


| Issue Type         | PM  | BrSE | Team Leader | Tech Leader | Developer | QC / Tester | BA / Designer |
| ------------------ | --- | ---- | ----------- | ----------- | --------- | ----------- | ------------- |
| `User Story`       | ✅   | ✅    | ✅           | ✅           | ❌         | ❌           | ❌             |
| `Customer Request` | ✅   | ✅    | ✅           | ✅           | ❌         | ❌           | ❌             |
| `Task`             | ✅   | ✅    | ✅           | ✅           | ✅         | ✅           | ✅             |
| `Bug`              | ✅   | ✅    | ✅           | ✅           | ✅         | ✅           | ✅             |
| `Internal Request` | ✅   | ✅    | ✅           | ✅           | ✅         | ✅           | ✅             |


---

### 3.3 Quyền Chuyển Trạng Thái → Closed


| Issue Type         | PM  | BrSE | Team Leader | Tech Leader | Developer | QC / Tester |
| ------------------ | --- | ---- | ----------- | ----------- | --------- | ----------- |
| `User Story`       | ✅   | ✅    | ✅           | ✅           | ❌         | ❌           |
| `Customer Request` | ✅   | ✅    | ✅           | ✅           | ❌         | ❌           |
| `Task`             | ✅   | ✅    | ✅           | ✅           | ❌         | ❌           |
| `Internal Request` | ✅   | ✅    | ✅           | ✅           | ❌         | ❌           |
| `Bug`              | ✅   | ✅    | ✅           | ✅           | ❌         | ✅           |


> ⚠️ **Lưu ý:**
>
> - Với tất cả loại **trừ Bug**: chỉ PM hoặc Leader mới được chuyển sang `Closed`.
> - Với **Bug**: QC verify xong → chuyển `Testing Request` → `Resolved` → `Closed`.

---

## IV. Quy Tắc Chung

1. **Không tạo issue trùng lặp** — kiểm tra trước khi tạo mới.
2. **Cập nhật Actual Hour** khi chuyển sang `Resolved`.
3. **Bug phải có Parent Issue** — link rõ ràng đến Story hoặc feature liên quan.
4. **Không tự ý đóng issue của người khác** nếu không có quyền.
5. **Customer Request phải được PM xác nhận** trước khi assign cho dev.
6. **Reopen phải có comment lý do** — ghi rõ tại sao issue bị mở lại.

---

## V. Lịch Sử Cập Nhật


| Version | Ngày | Người cập nhật | Nội dung thay đổi |
| ------- | ---- | -------------- | ----------------- |
| v1.0    | —    | PM             | Khởi tạo tài liệu |


---

*© Backlog Guideline — Tài liệu nội bộ dự án. Vui lòng không chia sẻ ra ngoài.*

---

## Doc Structure

Khi tạo task file (task-X-Y.md), xem `.claude/context/doc-structure.md` để biết cấu trúc folder.

**Path duy nhất:** `<DOCS_ROOT>/features/<feature-name>/<repo-name>/tasks/task-X-Y.md`.
</content>
