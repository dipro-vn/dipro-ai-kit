---
description: Phân tích feature đi qua nhiều module nối tiếp, xây Module Map + Dimension Catalog, sinh ma trận kết hợp (Pairwise/Business-critical/Full Cartesian) bằng script Python.
skills:
  - requirements_analyzer
---

> **Canonical workflow:** `.claude/agents/qc-agent.md`.
> **BẮT BUỘC:** Nạp skill `requirements_analyzer`.

# /test/generate_cross_module_test_plan

> **Dùng khi:** tính năng đi qua **nhiều modules nối tiếp**, output cuối phụ thuộc **tổ hợp dimensions** (ví dụ: Order với loại khách hàng × loại hợp đồng × phương thức thanh toán × loại giao hàng).

## Khi nào dùng workflow này?

| Tình huống | Dùng? |
|---|---|
| 1 module / 1 form | ❌ Dùng `/test/generate_manual_testcases_rbt` |
| Nhiều module **độc lập** | ⚠️ Sinh từng module riêng |
| Nhiều module **nối tiếp**, output phụ thuộc bộ điều kiện | ✅ **Đúng workflow này** |
| Cần ma trận Pairwise / Decision Table đa chiều | ✅ |

## Modes

| Mode | Khi nào | Input |
|---|---|---|
| **DOCUMENT** | Có SPEC.md mô tả modules + rules | SPEC.md path |
| **BROWSER** | Cần inspect DOM thực tế của ứng dụng đang chạy | URL + credentials |

Ưu tiên DOCUMENT khi đã có SPEC chuẩn BMAD.

## Input

| Input | Bắt buộc | Mô tả |
|---|---|---|
| Tên feature / luồng | ✅ | Ví dụ: "Đặt hàng có hợp đồng → giao hàng → thanh toán" |
| SPEC.md | ✅ | |
| Modules tham gia | ⚠️ Nên có | Nếu không → agent extract từ SPEC |
| Dimensions | ⚠️ Nên có | Nếu không → agent extract từ SPEC |
| Chiến lược ma trận | ❌ | `pairwise` (mặc định) · `business-critical` · `full-cartesian` |

## Output path

`docs/features/<feature>/test-cases/cross_module_plan.md`

## Các bước

### Bước 1 — Module Recon
Đọc SPEC, extract:
- Danh sách modules + thứ tự
- Fields/controls per module
- Giá trị có thể chọn

Output: **Module Inventory**

```
| # | Module | URL/Path | Inputs | Key Dimensions | Output | → Next |
```

### Bước 2 — Data Flow + Dimensions

```
| Module đích | Phụ thuộc | Trường | Loại phụ thuộc |
```

```
| # | Dimension | Module nguồn | Giá trị có thể | Số values |
```

Tính `Full Cartesian = D1 × D2 × ...` và liệt kê **constraints** (bộ không hợp lệ).

### Bước 3 — Sinh ma trận (CORE OUTPUT)

#### 3A. Pairwise (mặc định)

> **NGHIÊM CẤM** tính pairwise thủ công. PHẢI dùng script Python `allpairspy`.

```python
from allpairspy import AllPairs

dimensions = {
    "D1_customer_type": ["Corporate", "SME", "Startup"],
    "D2_contract_type": ["Monthly", "OneTime"],
    "D3_payment_method": ["CreditCard", "BankTransfer", "COD"],
}

def is_valid(row):
    # constraint: Startup không có Monthly contract
    if len(row) >= 2 and row[0] == "Startup" and row[1] == "Monthly":
        return False
    return True

values = list(dimensions.values())
keys = list(dimensions.keys())
print(f"| # | {' | '.join(keys)} |")
print(f"|{'---|' * (len(keys) + 1)}")
for i, combo in enumerate(AllPairs(values, filter_func=is_valid)):
    print(f"| {i+1} | {' | '.join(str(v) for v in combo)} |")
```

Nếu `allpairspy` chưa cài: `pip install allpairspy`.

#### 3B. Business-Critical (8-15 bộ)
Tiêu chí: phổ biến thực tế + liên quan tiền/thuế + edge giữa các loại.

#### 3C. Full Cartesian
Chỉ dùng khi tổng ≤ 50 bộ.

### Bước 4 — Đóng gói + (Optional) Sinh data

Output chính:
```markdown
## Ma Trận Kết Hợp (Pairwise — N bộ)
| # | D1 | D2 | D3 | ... | Risk |
```

Nếu user yêu cầu `--with-data`:
```json
{
  "combination_id": "COMBO_01",
  "dimensions": {"D1": "Corporate", "D2": "Monthly", "D3": "CreditCard"},
  "supporting_data": {
    "company_name": "auto_c01_<ts>",
    "tax_id": "0123456789",
    "amount": 100000000
  }
}
```

**DỪNG checkpoint** chờ user xác nhận ma trận.

## Bước tiếp

| Mục tiêu | Workflow tiếp |
|---|---|
| Sinh TC chi tiết cho từng bộ | `/test/generate_manual_testcases_rbt` |
| Sinh test data cho ma trận | `/test/generate_test_data --combinatorial` |

## NGHIÊM CẤM

| ❌ | ✅ |
|---|---|
| Tự tính pairwise thủ công | Sinh script Python + `allpairspy` |
| Đoán dimensions không có nguồn | Extract từ SPEC.md |
| Sinh Full Cartesian khi dimensions lớn | Pairwise giảm 80-90% |
| Bỏ qua constraints invalid | Loại bỏ qua `is_valid()` filter |
