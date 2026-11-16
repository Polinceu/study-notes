# Rust：错误处理，Result + ? 是主线

Rust 没有异常，可恢复的错误都用 `Result<T, E>` 返回，`?` 负责一路向上传。

## 基本形态

```rust
use std::fs;
use std::io;

fn read_config() -> Result<String, io::Error> {
    let text = fs::read_to_string("config.toml")?;  // 出错就直接 return Err
    Ok(text)
}

fn main() {
    match read_config() {
        Ok(text) => println!("{}", text),
        Err(e) => eprintln!("读配置失败：{}", e),
    }
}
```

## 两种错误的边界

- 能恢复的（文件不存在、网络超时）：返回 `Result`，让调用方决定。
- 程序员的错（数组越界、下标错误）：`panic!`，不该用 `Result` 包起来。

## main 函数也可以返回 Result

```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    let text = fs::read_to_string("config.toml")?;
    println!("{}", text);
    Ok(())
}
```

`Box<dyn Error>` 能装下各种错误类型，写小工具时最省事；库代码里一般定义自己的错误枚举。
