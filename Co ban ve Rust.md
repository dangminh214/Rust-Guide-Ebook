# RUST CƠ BẢN

## 1. Giới thiệu về Rust

Rust là một ngôn ngữ lập trình hệ thống được phát triển bởi Mozilla, tập trung vào độ an toàn bộ nhớ, hiệu suất cao và tính đồng thời. Từ khi ra mắt phiên bản 1.0 vào năm 2015, Rust đã nhanh chóng trở thành một trong những ngôn ngữ được yêu thích nhất theo khảo sát của Stack Overflow trong nhiều năm liên tiếp.

### Đặc điểm chính của Rust:

- **An toàn bộ nhớ không cần Garbage Collection**: Rust đảm bảo an toàn bộ nhớ tại thời điểm biên dịch mà không cần Garbage Collection.
- **Concurrency không có data race**: Hệ thống ownership và borrowing giúp ngăn chặn các lỗi đồng thời.
- **Zero-cost abstractions**: Các tính năng trừu tượng không ảnh hưởng đến hiệu suất.
- **Tính năng hiện đại**: Hệ thống kiểu mạnh mẽ, pattern matching, inference type, và nhiều tính năng khác.

## 2. Cài đặt Rust

Cách đơn giản nhất để cài đặt Rust là sử dụng Rustup - công cụ quản lý phiên bản Rust:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Sau khi cài đặt, bạn có thể kiểm tra phiên bản bằng lệnh:

```bash
rustc --version
cargo --version
```

## 3. Công cụ phát triển

- **rustc**: Trình biên dịch Rust
- **cargo**: Hệ thống quản lý gói và build tool
- **rustfmt**: Công cụ định dạng mã
- **clippy**: Công cụ phân tích mã tĩnh
- **rust-analyzer**: Language server cho các IDE

## 4. Cấu trúc dự án Rust với Cargo

Cargo là hệ thống quản lý gói và công cụ xây dựng chính thức của Rust. Nó cung cấp các tính năng quan trọng để phát triển và quản lý dự án Rust một cách hiệu quả:

- **Quản lý phụ thuộc**: Tự động tải xuống và cập nhật các thư viện (crates)
- **Xây dựng dự án**: Biên dịch và tối ưu mã nguồn
- **Testing và benchmarking**: Hỗ trợ kiểm thử tích hợp
- **Xuất bản crates**: Đăng tải thư viện lên crates.io
- **Quản lý workspace**: Tổ chức các dự án liên quan
- **Profiles**: Cấu hình biên dịch cho các môi trường khác nhau (debug/release)

Cargo được cài đặt mặc định khi bạn cài đặt Rust và giúp đơn giản hóa quy trình phát triển từ đầu đến cuối.

Tạo dự án mới:

```bash
cargo new ten_du_an
```

Cấu trúc dự án:
```
ten_du_an/
├── Cargo.toml  # File cấu hình dự án
└── src/
    └── main.rs  # File mã nguồn chính
```

Các lệnh Cargo cơ bản:
- `cargo build`: Biên dịch dự án
- `cargo run`: Biên dịch và chạy dự án
- `cargo test`: Chạy các bài kiểm tra
- `cargo check`: Kiểm tra lỗi không biên dịch
- `cargo doc`: Tạo tài liệu

## 5. Cú pháp cơ bản

### Biến và tính bất biến

Lưu ý: tất cả các biến trong Rust nếu được khai báo sẽ luôn có mặc định là immutable (không thể thay đổi)

```rust
// Biến mặc định là bất biến (immutable)
let x = 5;
// x = 6; // Lỗi!

// Biến có thể thay đổi (mutable)
let mut y = 5;
y = 6; // Hợp lệ
```

### Kiểu dữ liệu cơ bản

```rust
// Số nguyên: i8, i16, i32, i64, i128, u8, u16, u32, u64, u128
let so_nguyen: i32 = 42;

// Số thực: f32, f64
let so_thuc: f64 = 3.14;

// Boolean
let dung: bool = true;
let sai: bool = false;

// Ký tự và chuỗi
let ky_tu: char = 'A';
let chuoi: &str = "Xin chào";
let chuoi_string: String = String::from("Xin chào");

// Tuple
let tuple: (i32, f64, &str) = (42, 3.14, "Xin chào");

// Array
let mang: [i32; 5] = [1, 2, 3, 4, 5];
```

### Hàm

```rust
fn chao_hoi(ten: &str) -> String {
    format!("Xin chào, {}!", ten)
}

fn main() {
    let loi_chao = chao_hoi("Việt Nam");
    println!("{}", loi_chao);
}
```

### Cấu trúc điều kiện

```rust
fn main() {
    let so = 5;

    if so < 0 {
        println!("Số âm");
    } else if so == 0 {
        println!("Số không");
    } else {
        println!("Số dương");
    }

    // If là biểu thức, có thể gán giá trị
    let trang_thai = if so > 0 { "dương" } else { "không dương" };
}
```

### Vòng lặp

```rust
fn main() {
    // Vòng lặp for
    for i in 0..5 {
        println!("Giá trị: {}", i);
    }

    // Vòng lặp while
    let mut n = 0;
    while n < 5 {
        println!("n = {}", n);
        n += 1;
    }

    // Vòng lặp loop
    let mut count = 0;
    let result = loop {
        count += 1;
        if count == 10 {
            break count * 2; // Trả về giá trị khi thoát loop
        }
    };
}
```

## 6. Ownership - Tính năng đặc trưng của Rust

Ownership là một trong những khái niệm quan trọng nhất của Rust, giúp đảm bảo an toàn bộ nhớ mà không cần garbage collector.

### Quy tắc ownership:
1. Mỗi giá trị trong Rust có một biến gọi là owner
2. Chỉ có thể có một owner tại một thời điểm
3. Khi owner ra khỏi phạm vi, giá trị sẽ bị hủy

```rust
fn main() {
    // s1 là owner của giá trị chuỗi
    let s1 = String::from("xin chào");

    // Move ownership từ s1 sang s2
    let s2 = s1;

    // println!("{}", s1); // Lỗi! s1 không còn hợp lệ

    // Clone tạo bản sao, giữ nguyên owner cũ
    let s1 = String::from("xin chào");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2); // Hợp lệ
}
```

### Borrowing

Borrowing cho phép tham chiếu đến giá trị mà không lấy ownership:

```rust
fn main() {
    let s1 = String::from("xin chào");

    // Tham chiếu bất biến
    let len = tinh_do_dai(&s1);
    println!("Độ dài của '{}' là {}.", s1, len);

    // Tham chiếu có thể thay đổi
    let mut s = String::from("xin");
    them_chuoi(&mut s);
    println!("{}", s); // "xin chào"
}

fn tinh_do_dai(s: &String) -> usize {
    s.len()
}

fn them_chuoi(s: &mut String) {
    s.push_str(" chào");
}
```

## 7. Struct và Enum

### Struct

```rust
// Định nghĩa struct
struct NguoiDung {
    ten: String,
    email: String,
    so_diem: u32,
    hoat_dong: bool,
}

fn main() {
    // Tạo instance của struct
    let user1 = NguoiDung {
        ten: String::from("Nguyễn Văn A"),
        email: String::from("a@example.com"),
        so_diem: 100,
        hoat_dong: true,
    };

    // Truy cập các trường
    println!("Tên: {}", user1.ten);

    // Struct với implementation
    impl NguoiDung {
        // Constructor
        fn new(ten: String, email: String) -> NguoiDung {
            NguoiDung {
                ten,
                email,
                so_diem: 0,
                hoat_dong: false,
            }
        }

        // Method
        fn chao_hoi(&self) -> String {
            format!("Xin chào, {}!", self.ten)
        }
    }

    let user2 = NguoiDung::new(
        String::from("Trần Thị B"),
        String::from("b@example.com")
    );
    println!("{}", user2.chao_hoi());
}
```

### Enum

```rust
enum TrangThaiDonHang {
    DangXuLy,
    DangGiao { ma_van_don: String },
    DaGiao(String), // Thời gian giao hàng
    Huy,
}

fn main() {
    let don_hang = TrangThaiDonHang::DangGiao {
        ma_van_don: String::from("VN123456")
    };

    // Pattern matching với match
    match don_hang {
        TrangThaiDonHang::DangXuLy => {
            println!("Đơn hàng đang xử lý");
        }
        TrangThaiDonHang::DangGiao { ma_van_don } => {
            println!("Đơn hàng đang giao với mã vận đơn: {}", ma_van_don);
        }
        TrangThaiDonHang::DaGiao(thoi_gian) => {
            println!("Đơn hàng đã giao lúc: {}", thoi_gian);
        }
        TrangThaiDonHang::Huy => {
            println!("Đơn hàng đã bị hủy");
        }
    }
}
```

## 8. Option và Result

Rust không có `null`, thay vào đó sử dụng enum `Option` và `Result` để xử lý các giá trị có thể không tồn tại hoặc lỗi.

### Option

```rust
fn main() {
    let so: Option<i32> = Some(5);
    let khong_co: Option<i32> = None;

    // Unwrap có thể gây panic nếu là None
    println!("Giá trị: {}", so.unwrap());

    // Cách an toàn hơn với match
    match so {
        Some(gia_tri) => println!("Có giá trị: {}", gia_tri),
        None => println!("Không có giá trị"),
    }

    // Hoặc dùng if let
    if let Some(gia_tri) = so {
        println!("Có giá trị: {}", gia_tri);
    }
}
```

### Result

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    // Result<T, E> dùng để xử lý lỗi
    let ket_qua = File::open("hello.txt");

    // Xử lý Result với match
    let file = match ket_qua {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Không thể tạo file: {:?}", e),
            },
            other_error => panic!("Lỗi mở file: {:?}", other_error),
        },
    };

    // Cách ngắn gọn hơn
    let file = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Không thể tạo file: {:?}", error);
            })
        } else {
            panic!("Lỗi mở file: {:?}", error);
        }
    });
}
```

## 9. Traits

Traits trong Rust tương tự như interface trong các ngôn ngữ khác:

```rust
// Định nghĩa trait
trait HanhDong {
    fn mo_ta(&self) -> String;

    // Phương thức mặc định
    fn thuc_hien(&self) -> String {
        format!("Đang thực hiện: {}", self.mo_ta())
    }
}

struct Di {
    khoang_cach: u32,
}

struct Noi {
    noi_dung: String,
}

// Triển khai trait cho struct Di
impl HanhDong for Di {
    fn mo_ta(&self) -> String {
        format!("Di chuyển {} mét", self.khoang_cach)
    }
}

// Triển khai trait cho struct Noi
impl HanhDong for Noi {
    fn mo_ta(&self) -> String {
        format!("Nói: {}", self.noi_dung)
    }

    // Ghi đè phương thức mặc định
    fn thuc_hien(&self) -> String {
        format!("ĐANG NÓI TO: {}", self.noi_dung)
    }
}

fn main() {
    let hanh_dong_di = Di { khoang_cach: 10 };
    let hanh_dong_noi = Noi { noi_dung: String::from("Xin chào") };

    println!("{}", hanh_dong_di.mo_ta());
    println!("{}", hanh_dong_di.thuc_hien());

    println!("{}", hanh_dong_noi.mo_ta());
    println!("{}", hanh_dong_noi.thuc_hien());

    // Sử dụng trait bound trong hàm
    fn in_hanh_dong<T: HanhDong>(item: &T) {
        println!("Hành động: {}", item.mo_ta());
    }

    in_hanh_dong(&hanh_dong_di);
    in_hanh_dong(&hanh_dong_noi);
}
```

## 10. Quản lý lỗi

Rust khuyến khích xử lý lỗi rõ ràng thông qua `Result`:

```rust
use std::fs::File;
use std::io::{self, Read};

// Trả về Result từ hàm
fn doc_noi_dung_file(path: &str) -> Result<String, io::Error> {
    let mut file = File::open(path)?; // ? trả về lỗi nếu mở file thất bại
    let mut noi_dung = String::new();
    file.read_to_string(&mut noi_dung)?; // ? trả về lỗi nếu đọc file thất bại
    Ok(noi_dung)
}

fn main() {
    match doc_noi_dung_file("hello.txt") {
        Ok(noi_dung) => println!("Nội dung file: {}", noi_dung),
        Err(e) => println!("Lỗi: {}", e),
    }

    // Hoặc dùng unwrap() trong những trường hợp đơn giản
    // (cẩn thận với panic)
    let noi_dung = doc_noi_dung_file("hello.txt").unwrap();

    // Hoặc expect() để cung cấp thông báo lỗi tùy chỉnh
    let noi_dung = doc_noi_dung_file("hello.txt")
        .expect("Không thể đọc file hello.txt");
}
```

## 11. Modules và Crates

Rust tổ chức mã nguồn thành modules, crates và packages.

```rust
// lib.rs
mod dong_vat {
    pub struct Cho {
        pub ten: String,
        tuoi: u8,
    }

    impl Cho {
        pub fn new(ten: String, tuoi: u8) -> Cho {
            Cho { ten, tuoi }
        }

        pub fn sua(&self) -> String {
            format!("{} sủa: Gâu gâu!", self.ten)
        }
    }

    pub mod chim {
        pub fn hot() {
            println!("Chíp chíp!");
        }
    }
}

// Sử dụng module
use crate::dong_vat::Cho;
use crate::dong_vat::chim::hot;

pub fn chay() {
    let cho = Cho::new(String::from("Lu Lu"), 3);
    println!("{}", cho.sua());

    hot();
}
```

## 12. Concurrency

Rust cung cấp các công cụ mạnh mẽ để lập trình đồng thời an toàn.

```rust
use std::thread;
use std::time::Duration;
use std::sync::{Arc, Mutex};

fn main() {
    // Thread đơn giản
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("Hi từ thread con: {}", i);
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("Hi từ thread chính: {}", i);
        thread::sleep(Duration::from_millis(1));
    }

    // Đợi thread con hoàn thành
    handle.join().unwrap();

    // Chia sẻ dữ liệu giữa các thread với Arc và Mutex
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();
            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Kết quả: {}", *counter.lock().unwrap());
}
```

## 13. Macros

Macros là một cách mạnh mẽ để viết mã tạo mã trong Rust.

```rust
// Macro đơn giản
macro_rules! in_xin_chao {
    () => {
        println!("Xin chào!");
    };
    ($ten:expr) => {
        println!("Xin chào, {}!", $ten);
    };
}

fn main() {
    in_xin_chao!(); // In: Xin chào!
    in_xin_chao!("Rust"); // In: Xin chào, Rust!
}
```

## 14. Tổng kết

Rust là một ngôn ngữ mạnh mẽ với nhiều ưu điểm:
- An toàn bộ nhớ không cần garbage collection
- Hiệu suất cao, có thể so sánh với C/C++
- Tính năng hiện đại từ ngôn ngữ cấp cao
- Cộng đồng phát triển mạnh mẽ
- Hệ sinh thái package ngày càng phong phú

Tuy vậy, Rust cũng có một số thách thức:
- Đường cong học tập dốc
- Hệ thống ownership và borrowing đôi khi phức tạp
- Thời gian biên dịch có thể dài

Rust phù hợp cho nhiều loại ứng dụng:
- Phát triển hệ thống
- Ứng dụng CLI
- WebAssembly
- Dịch vụ web hiệu suất cao
- IoT và nhúng
- Blockchain và tiền điện tử
