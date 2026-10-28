# Rust：生命周期省略规则（三条就够）

## 先看一个报错

```rust
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() { x } else { y }
}
// 报错：missing lifetime specifier
```

返回的引用可能来自 x 也可能来自 y，
编译器不知道它能活多久，只能让你显式标注：

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

`'a` 的意思是"取 x、y 中活得短的那个"，调用方保证即可。

## 三条省略规则（编译器自动推导）

```rust
fn first(s: &str) -> &str { &s[0..1] }  // 能编译，不用写 'a
```

1. 每个引用参数各得一个独立的生命周期。
2. 如果只有一个输入引用，输出就用它的。
3. 如果有 `&self`，输出的生命周期跟 `self` 走。

`first` 命中第 2 条，所以不用写。

## 什么时候必须手写

- 输入有**两个以上**引用，输出又借用了其中之一（上面的 longest）。
- 结构体里存引用：`struct R<'a> { s: &'a str }`。

新手期记住：报错让加 `'a` 就加，别跟编译器较劲，
它只是在帮你避开悬垂引用。
