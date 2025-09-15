# 🦀 Rust Ownership, Borrowing và Lifetime – Nâng Cao

---

## 1. Ôn lại kiến thức cơ bản

Trước khi đi sâu, nhắc lại một số nguyên tắc cơ bản:

- Ownership: Mỗi giá trị trong Rust chỉ có một chủ sở hữu (owner) tại một thời điểm.

- Borrowing: Thay vì chuyển quyền sở hữu, ta có thể mượn (borrow) bằng tham chiếu (&T hoặc &mut T).

- Lifetime: Thời gian sống (scope) của một tham chiếu. Rust cần biết lifetime để đảm bảo không có tham chiếu treo (dangling reference).

---

## 2. Borrowing nâng cao

### 2.1. Quy tắc bất biến & khả biến

Rust cho phép:

- Nhiều borrow bất biến (&T) cùng lúc.

- Một borrow khả biến (&mut T) duy nhất tại một thời điểm.

```rust
fn main() {
    let mut x = 10;

    let r1 = &x; // bất biến
    let r2 = &x; // bất biến
    println!("r1: {}, r2: {}", r1, r2);

    // let r3 = &mut x; // ❌ lỗi: không thể borrow mut khi đang có borrow bất biến
}
```
Mẹo nâng cao:
Rust cho phép "non-lexical lifetimes" (NLL) — borrow kết thúc sớm khi tham chiếu không còn được dùng.

### 2.2. Borrowing từng phần (Field-level Borrowing)

Rust cho phép borrow riêng từng field trong struct nếu chúng không chồng chéo.

```rust
struct Point { x: i32, y: i32 }

fn main() {
    let mut p = Point { x: 1, y: 2 };

    let x_ref = &mut p.x;
    *x_ref += 10;

    let y_ref = &p.y; // ✅ hợp lệ: borrow field khác
    println!("x={}, y={}", x_ref, y_ref);
}
```

### 2.3. Interior Mutability (Cell, RefCell)

Nếu cần thay đổi giá trị ngay cả khi đang có nhiều tham chiếu bất biến, ta dùng interior mutability.

```rust
use std::cell::RefCell;

fn main() {
    let x = RefCell::new(5);

    let b1 = x.borrow_mut();
    *b1 += 1;

    let b2 = x.borrow();
    println!("x = {}", *b2);
}
```
> ⚠️ RefCell chỉ kiểm tra borrow lúc runtime, có thể panic nếu vi phạm quy tắc borrow.

---

## 3. Lifetime nâng cao

### 3.1. Hàm trả về tham chiếu

Rust cần biết lifetime của tham số để suy ra lifetime của giá trị trả về.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

fn main() {
    let s1 = String::from("Hello");
    let s2 = "World";

    let result = longest(&s1, s2);
    println!("{}", result);
}
```
- `'a` là lifetime parameter, cho biết giá trị trả về sống ít nhất bằng lifetime của tham số ngắn nhất.

### 3.2. Struct chứa tham chiếu

Khi struct chứa tham chiếu, cần khai báo lifetime.

```rust
struct Book<'a> {
    title: &'a str,
}

fn main() {
    let t = String::from("Rust Book");
    let b = Book { title: &t };
    println!("Book: {}", b.title);
}
```

### 3.3. Lifetime Subtyping

Nếu một hàm nhận nhiều tham chiếu với lifetime khác nhau, Rust sẽ cố gắng "thu hẹp" lifetime để an toàn.

```rust
fn foo<'a, 'b: 'a>(x: &'a str, y: &'b str) -> &'a str {
    println!("{}", y);
    x
}
```

Ở đây `'b: 'a` nghĩa là `'b` sống ít nhất bằng `'a`.

### 3.4. Hạn chế lifetime khi dùng closure

Closure có thể giữ tham chiếu và làm lifetime phức tạp hơn.

```rust
fn make_closure<'a>(x: &'a str) -> impl Fn() -> &'a str {
    move || x
}
```

### 3.5. 'static Lifetime

- `'static` nghĩa là tham chiếu tồn tại suốt chương trình (ví dụ: string literal).

- Cẩn thận khi convert tham chiếu thành `'static` bằng `Box::leak` hoặc `lazy_static!`, có thể gây rò rỉ bộ nhớ.

```rust
fn main() {
    let s: &'static str = "Tồn tại suốt chương trình";
    println!("{}", s);
}
```

---

## 4. Pattern nâng cao

### 4.1. Returning Iterator với Lifetime

```rust
fn split_words<'a>(text: &'a str) -> impl Iterator<Item = &'a str> {
    text.split_whitespace()
}
```

### 4.2. PhantomData & Lifetime Marker

Khi cần gắn lifetime cho struct mà không chứa tham chiếu trực tiếp.

```rust
use std::marker::PhantomData;

struct Wrapper<'a, T> {
    value: *const T,
    _marker: PhantomData<&'a T>,
}
```

---

## 5. Best Practices

- Dùng `Rc<RefCell<T>>` hoặc `Arc<Mutex<T>>` khi cần chia sẻ dữ liệu và thay đổi ở nhiều nơi.
- Tránh lifetime parameter không cần thiết – dùng `impl Trait` khi có thể.
- Nếu hàm phức tạp, có thể clone dữ liệu để giảm rắc rối lifetime (trade-off giữa hiệu năng và đơn giản).