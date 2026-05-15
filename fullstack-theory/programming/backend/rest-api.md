# REST API

## What is RESTful API?
> RESTful API is an API design style where we model the system around resources. Each resource is identified by a URL, and we use standard HTTP methods like GET, POST, PUT, PATCH, and DELETE to operate on those resources. A good REST API should be stateless, predictable, use proper status codes, support versioning, and return consistent response structures. This makes the API easier to understand, cache, scale, and maintain.

## Core idea
URL đại diện cho resource, HTTP method đại diện cho hành động, HTTP status code đại diện cho kết quả.

## REST giải quyết bài toán gì?
Khi hệ thống backend lớn lên, nếu mỗi team tự đặt API theo ý mình, API sẽ rất hỗn loạn:
```
/getUserInfo
/fetchUser
/loadUserData
/createNewUser
/removeUser
/deleteUserById
```
### Vấn đề:

- Khó đoán API làm gì
- Khó maintain
- Khó onboard developer mới
- Khó versioning
- Khó chuẩn hóa error handling

Frontend/mobile phải nhớ quá nhiều convention riêng

### Ví dụ resource:
```
/users
/orders
/products
```
HTTP method là thao tác với tài liệu:
```
GET     = đọc
POST    = tạo mới
PUT     = thay thế toàn bộ
PATCH   = cập nhật một phần
DELETE  = xóa
```
---
## Quan trọng:
| Method |  Safe |         Idempotent | Ý nghĩa                     |
| ------ | ----: | -----------------: | --------------------------- |
| GET    |    Có |                 Có | Đọc dữ liệu                 |
| POST   | Không |     Không mặc định | Tạo mới hoặc trigger action |
| PUT    | Không |                 Có | Replace toàn bộ             |
| PATCH  | Không | Không luôn đảm bảo | Update một phần             |
| DELETE | Không |                 Có | Xóa resource                |

Sao lại nói PATCH không luôn đảm bảo Idempotent? Ví dụ:
```
PATCH /users/123
{
  "name": "Alice"
}
```
> Gọi nhiều lần sẽ không thay đổi state server nếu chỉ set field về giá trị cụ thể.
```
PATCH /carts/123
{
    "operation": "add_item",
    "item_id":  456,
    "quantity": 1
}
```
> Gọi nhiều lần sẽ làm state thay đổi nhiều lần, nên không idempotent.

### Safe khác gì Idempotent?

#### Safe nghĩa là không thay đổi state server.

Ví dụ:
```
GET /users/123
```
> Không nên thay đổi dữ liệu.

### Stateless
REST yêu cầu server nên stateless - mỗi request phải chứa đủ thông tin để server xử lý.

Ví dụ request cần có:
```
Authorization: Bearer <token>
Content-Type: application/json
```

Server không nên phụ thuộc vào memory local kiểu:
- User A đang login ở server instance 1
- Cart của user A đang lưu trong RAM server instance 1

Vì khi scale horizontally, request tiếp theo của user có thể đi vào server khác:
```
Client
  |
Load Balancer
  |
  +--> Server 1
  +--> Server 2
  +--> Server 3
```

> Nếu state nằm trong RAM của một server, hệ thống sẽ lỗi hoặc phải dùng [sticky session](sticky-session.md).

RESTful API tốt thường để state ở:
```
Database
Redis
JWT / session store
Distributed cache
Object storage
```

## RESTful API Design
- Có thể **nested resource** nhưng không nên quá sâu - sẽ khó maintain. Thường 2 level là đủ.
- Path Params dùng để định danh
- Query Params dùng để filter, sort, search, [pagination](pagination.md)
- Request Body dùng để gửi data khi cần create/update resource.

## HTTP Status Codes
- Có chuẩn [HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

## Response Format
- Nên nhất quán, thường là JSON.
- System lớn, đặc biệt distributed systems thì nên có kèm cả requestId để trace log.

## Versioning API
