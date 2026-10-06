# Rust：所有权与 move，一次讲清

Rust 没有 GC，靠"所有权"在编译期管内存。核心规则就一条：
**每个值同一时间只有一个所有者，所有权转移后旧变量失效。**

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;              // 所有权 move 给 s2
    // println!("{}", s1);    // 编译报错：s1 已经失效
    println!("{}", s2);       // ok
}
```

`String` 的内容在堆上，move 只转交指针，不做深拷贝。
如果旧变量还能用，释放时就会 double free，所以编译器直接不让用。

## 想继续用旧变量？两种办法

```rust
let s1 = String::from("hello");
let s2 = s1.clone();   // 深拷贝，贵，但 s1 还能用
takes_ref(&s1);         // 借用：只借不拿，所有权还在 s1 手里
```

## Copy 类型不受影响

```rust
let x = 5;
let y = x;              // i32 实现了 Copy，纯拷贝
println!("{} {}", x, y); // x 照常用
```

整数、bool、char、浮点这些栈上小类型都是 Copy，
赋值就是拷贝，没有 move 一说。

一句话：堆上大数据 move 交接，栈上小数据 copy 随便用。
