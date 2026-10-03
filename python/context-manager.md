# Python：with 背后是上下文管理器

`with open(...)` 用了无数次，今天自己实现了一个计时器，才算真懂。

## 最小实现：两个魔术方法

```python
import time

class Timer:
    def __enter__(self):
        self.start = time.time()
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"耗时: {time.time() - self.start:.2f}s")
        return False  # 不吞异常

with Timer():
    sum(range(10**7))
# 耗时: 0.21s
```

`__exit__` 返回 True 会吞掉异常，一般别这么干。

## 偷懒写法：contextlib

```python
from contextlib import contextmanager

@contextmanager
def timer():
    start = time.time()
    yield
    print(f"耗时: {time.time() - start:.2f}s")

with timer():
    sum(range(10**7))
```

`yield` 之前是 `__enter__`，之后是 `__exit__`，代码量少一半。

## 什么时候值得写一个

需要"成对出现"的资源操作：加锁/解锁、计时、临时切换目录。
如果只是开关文件，直接用内置的 open 就行。
