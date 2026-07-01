## Yêu cầu

- macOS (M1/M2/M3/M4/M5)
- Tài khoản AWS IAM đã có quyền `ssm:StartSession`
- Access Key & Secret Key từ Lead/DevOps

---

## Bước 1 — Cài AWS CLI

```bash
brew install awscli

```

Kiểm tra:

```bash
aws --version

```

> Nếu chưa có Homebrew: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

---

## Bước 2 — Cài Session Manager Plugin

```bash
curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/mac_arm64/session-manager-plugin.pkg" \
  -o "session-manager-plugin.pkg"

sudo installer -pkg session-manager-plugin.pkg -target /

sudo mkdir -p /usr/local/bin && \
sudo ln -s /usr/local/sessionmanagerplugin/bin/session-manager-plugin /usr/local/bin/session-manager-plugin

```

Kiểm tra:

```bash
session-manager-plugin --version

```

---

## Bước 3 — Cấu hình AWS Profile

```bash
aws configure --profile <aws-profile-name>

```

Điền lần lượt:

```auto
AWS Access Key ID:     <xin từ PM>
AWS Secret Access Key: <xin từ PM>
Default region name:   <xin từ PM>
Default output format: json

```

Kiểm tra profile hoạt động:

```bash
aws sts get-caller-identity --profile <aws-profile-name>

```

✅ Thành công khi thấy trả về `Account`, `UserId`, `Arn`.

---

## Bước 4 — Cài DBeaver

Tải tại: https://dbeaver.io/download/

> MySQL Workbench **không hỗ trợ PostgreSQL** — phải dùng DBeaver hoặc TablePlus.

---

## Bước 5 — Mở tunnel SSM

Mở terminal, chạy lệnh sau và **giữ nguyên không tắt**:

```bash
aws --profile <aws-profile-name> \
  ssm start-session \
  --target <ec2-instance-id> \
  --document-name AWS-StartPortForwardingSessionToRemoteHost \
  --parameters '{"host":["<staging-db-host>"],"portNumber":["5432"],"localPortNumber":["<local-port>"]}'

```

✅ Thành công khi terminal hiển thị: `Waiting for connections...`

> Điền `<ec2-instance-id>`, `<staging-db-host>`, `<local-port>` bằng giá trị thật của dự án (xin từ Lead/DevOps).

---

## Bước 6 — Connect DBeaver

1. Mở DBeaver → **Database** → **New Database Connection**
2. Chọn **PostgreSQL** → Next
3. Điền thông tin:

| Field    | Giá trị           |
| -------- | ----------------- |
| Host     | `127.0.0.1`       |
| Port     | `<local-port>` (port local đã dùng ở Bước 5) |
| Database | `<db-name>` (xin từ PM) |
| Username | `<db-username>` (xin từ PM) |
| Password | `<db-password>` (xin từ PM) |

4. Nhấn **Test Connection** → **Finish**

> ⚠️ Host phải là `127.0.0.1` (không phải host RDS trực tiếp) ⚠️ Port phải khớp với `<local-port>` khai báo ở Bước 5

---

## Mỗi lần dùng

```auto
1. Mở terminal → chạy lệnh SSM (Bước 5)
2. Mở DBeaver → connect
3. Làm việc xong → Ctrl+C terminal để đóng tunnel

```

---

## Gặp lỗi?

| Lỗi                                 | Nguyên nhân                       | Fix                   |
| ----------------------------------- | ---------------------------------- | --------------------- |
| `command not found: aws`            | Chưa cài AWS CLI                  | Bước 1                |
| `profile not found`                 | Chưa configure profile            | Bước 3                |
| `AccessDeniedException`             | Không có quyền SSM                | Nhờ CTO add quyền IAM |
| `SessionManagerPlugin is not found` | Chưa cài plugin                   | Bước 2                |
| `Connection refused`                | Sai local port (không khớp Bước 5) | Sửa Port cho khớp `<local-port>` |
