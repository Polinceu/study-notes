# Go：errors.Is / errors.As，错误包了几层也能认出来

`fmt.Errorf("...: %w", err)` 包一层之后，`err == io.EOF` 就失效了，
因为外层是个新错误，`errors.Is` 是专门用来穿透包装的。

## Is：认"是不是这个错误"

```go
var ErrNotFound = errors.New("not found")

func load() error {
    return fmt.Errorf("load user: %w", ErrNotFound)
}

err := load()
errors.Is(err, ErrNotFound)  // true，穿透了一层包装
err == ErrNotFound           // false，别这么比
```

## As：取出某一层的具体类型

```go
var pathErr *os.PathError
if errors.As(err, &pathErr) {
    fmt.Println("出问题的文件：", pathErr.Path)
}
```

## 配套习惯

- 哨兵错误用包级 `var ErrXxx = errors.New(...)` 定义一次。
- 包装用 `%w`，只做信息补充用 `%v`（`%v` 包完就认不出来了）。
- 自定义错误类型想支持 `Is`，给它写 `Is(target error) bool` 方法。
