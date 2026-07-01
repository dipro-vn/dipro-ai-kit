# Coding Style

## Chung (tất cả repo)

- **Tối giản**: Không thêm feature ngoài yêu cầu. Không abstract sớm khi chưa có 3+ use case
- **Nhất quán**: Ưu tiên follow pattern đang có trong codebase — không tự refactor phạm vi ngoài task
- **Đặt tên rõ nghĩa**: Mỗi function/method làm 1 việc, tên nói lên đó là việc gì
- **Không dead code**: Không comment-out code, không unused import, không unused variable
- **Không comment mô tả WHAT**: Chỉ comment khi WHY là non-obvious (constraint ẩn, workaround bug cụ thể)

## NestJS (repo vai trò backend)

```typescript
// ✅ Column: snake_case explicit name
@Column({ name: 'company_code', length: 50 })
companyCode: string;

// ✅ Relation: eager: false để tránh N+1
@ManyToOne(() => CompanyEntity, { eager: false })
@JoinColumn({ name: 'company_id' })
company: CompanyEntity;

// ✅ orderBy whitelist — đã có bug prod từ việc này
const ORDER_BY_MAP = { createdAt: 'order.createdAt', status: 'order.status' };
.orderBy(ORDER_BY_MAP[dto.orderBy] ?? 'order.createdAt', 'DESC')
```

## React (repo vai trò frontend)

```tsx
// ✅ Named export + Props interface
interface OrderCardProps { orderId: string; }
export const OrderCard: React.FC<OrderCardProps> = ({ orderId }) => { ... };

// ✅ TanStack Query v5 — object syntax BẮT BUỘC
const { data } = useQuery({ queryKey: ['orders', id], queryFn: () => api.get(id) });

// ❌ v4 positional syntax — sai
const { data } = useQuery(['orders'], () => api.get()); // KHÔNG dùng
```

## Flutter (repo vai trò mobile)

```dart
// ✅ Riverpod — StateNotifierProvider
// ✅ Sizing dùng flutter_screenutil
Container(width: 100.w, height: 50.h) // KHÔNG hard-code pixel

// ✅ const constructor khi widget không đổi
const Text('Hello') // tiết kiệm rebuild
```
