# Kết nối PostgreSQL DEV

---

## Bước 1 — Cài DBeaver

Tải tại: https://dbeaver.io/download/ → cài như app bình thường.

---

## Bước 2 — Tạo connection mới

1. Mở DBeaver
2. Click **Database** → **New Database Connection**
3. Chọn **PostgreSQL** → **Next**

---

## Bước 3 — Nhập thông tin

| Field    | Giá trị                                                |
| -------- | ------------------------------------------------------ |
| Host     | `<db-dev-host>` (điền giá trị thật của dự án — ví dụ RDS endpoint) |
| Port     | `5432`                                                 |
| Database | `<db-name>` (điền giá trị thật)                        |
| Username | `<db-username>` (điền giá trị thật)                    |
| Password | `<db-password>` (lấy từ Lead/DevOps — không hard-code trong doc) |

---

## Bước 4 — Connect

Nhấn **Test Connection** → thấy **Connected** ✅ → nhấn **Finish**.

---

> Lần đầu DBeaver có thể hỏi tải driver PostgreSQL → nhấn **Download** là tự động cài.
