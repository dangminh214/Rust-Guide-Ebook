# Quản lý bộ nhớ Ownership trong Rust

## Giới thiệu
Ownership là một khái niệm quan trọng trong Rust, nó giúp quản lý bộ nhớ một cách hiệu quả và an toàn. Ownership được thiết kế để tránh các lỗi phổ biến như double free, use after free, và memory leaks.

## Các khái niệm cơ bản
Ownership trong Rust được quản lý thông qua các quyền sở hữu (ownership) và quyền truy cập (borrowing). Mỗi giá trị trong Rust chỉ có một quyền sở hữu duy nhất, và quyền sở hữu này không thể được chia sẻ giữa các biến khác.

## Các quy tắc Ownership
- Mỗi giá trị trong Rust chỉ có một quyền sở hữu duy nhất.
- Khi một giá trị được tạo ra, nó sẽ được sở hữu bởi biến đầu tiên được gán giá trị đó.
- Khi một giá trị được chuyển nhượng (move) từ một biến sang một biến khác, quyền sở hữu của giá trị đó sẽ được chuyển nhượng sang biến mới.
- Khi một giá trị được tạo ra trong một hàm, nó sẽ được giải phóng khi hàm kết thúc.
- Khi một giá trị được giải phóng, bộ nhớ được giải phóng theo chính sách của Rust.

Ownership trong Rust được quản lý thông qua các quyền sở hữu (ownership) và quyền truy cập (borrowing). Mỗi giá trị trong Rust chỉ có một quyền sở hữu duy nhất, và quyền sở hữu này không thể được chia sẻ giữa các biến khác.
