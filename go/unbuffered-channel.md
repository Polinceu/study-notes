# Go：无缓冲 channel 是"交接"，不是"队列"

## 发送和接收必须同时就绪

```go
package main

import "fmt"

func main() {
    ch := make(chan int)  // 无缓冲
    go func() {
        ch <- 42          // 阻塞，直到有人接收
    }()
    fmt.Println(<-ch)     // 42
}
```

如果没人接收，`ch <- 42` 会一直阻塞；
反过来没人发送，`<-ch` 也会一直阻塞。两边必须"碰头"。

## 有缓冲 vs 无缓冲

```go
ch2 := make(chan int, 2)
ch2 <- 1  // 不阻塞，缓冲区还有位置
ch2 <- 2  // 不阻塞
// ch2 <- 3  // 阻塞！缓冲区满了
```

## 生产者-消费者小例子

```go
jobs := make(chan int, 5)
done := make(chan bool)

go func() {
    for j := range jobs {
        fmt.Println("处理", j)
    }
    done <- true
}()

for i := 1; i <= 3; i++ {
    jobs <- i
}
close(jobs)   // 告诉消费者"没活了"
<-done
```

记住 `close`：range channel 会一直等，不 close 就死锁。
发送方负责 close，别让接收方 close。
