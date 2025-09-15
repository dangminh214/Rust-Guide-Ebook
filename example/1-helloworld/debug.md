# Debug

```rust
#[derive(Debug)]
struct Person<'a> { // declare a struct 🦊
    name: &'a str,
    age: u8
}

fn main() {
    let name = "Peter";
    let age = 27;
    let peter = Person { name, age };

    // Pretty print
    println!("{:?}", peter); // Person { name: "Peter", age: 27 } 🐻
    println!("{:#?}", peter);
    /*
    Person {
        name: "Peter",
        age: 27,
    }
    */
    println!("{}", peter.name); // Peter 🐦‍🔥
    println!("{}", peter.age); // 27 🪼
}
```