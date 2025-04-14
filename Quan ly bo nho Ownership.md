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

## Ví dụ về Ownership

### Ví dụ 1: Chuyển nhượng quyền sở hữu (Move)

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // s1 không còn hợp lệ sau dòng này

    // println!("{}", s1); // Lỗi: giá trị đã bị move
    println!("{}", s2); // OK
}
```

### Ví dụ 2: Copy với kiểu dữ liệu nguyên thủy

```rust
fn main() {
    let x = 5;
    let y = x; // x vẫn hợp lệ vì kiểu i32 có trait Copy

    println!("x = {}, y = {}", x, y); // Cả hai đều hoạt động bình thường
}
```

### Ví dụ 3: Ownership với hàm

```rust
fn main() {
    let s = String::from("hello");
    takes_ownership(s); // s bị move vào hàm
    // println!("{}", s); // Lỗi: s không còn hợp lệ

    let x = 5;
    makes_copy(x); // x được copy vào hàm
    println!("{}", x); // x vẫn hợp lệ
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string);
} // some_string bị drop ở đây

fn makes_copy(some_integer: i32) {
    println!("{}", some_integer);
} // some_integer bị drop nhưng không ảnh hưởng tới x
```

### Ví dụ 4: Trả về quyền sở hữu

```rust
fn main() {
    let s1 = gives_ownership(); // gives_ownership chuyển quyền sở hữu sang s1

    let s2 = String::from("hello");
    let s3 = takes_and_gives_back(s2); // s2 bị move vào hàm, và hàm trả về quyền sở hữu cho s3
}

fn gives_ownership() -> String {
    let some_string = String::from("hello");
    some_string // trả về quyền sở hữu
}

fn takes_and_gives_back(a_string: String) -> String {
    a_string // trả về quyền sở hữu
}
```
