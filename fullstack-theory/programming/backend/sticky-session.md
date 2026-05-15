# Sticky session

- Sticky session là cơ chế để Load Balancer luôn gửi request của cùng một user/session về cùng một server instance.
- Nó thường dùng khi server lưu state trong local memory, ví dụ: login session, cart, temporary data.
- Bản chất: sticky session giúp "né" vấn đề stateless, nhưng làm hệ thống khó scale, khó failover, dễ lệch tải hơn.

_Important:_ Trong backend hiện đại, thường ưu tiên stateless app + shared storage như Redis/DB hơn sticky session.
