# Python：类型注解写到什么程度才够用

注解不是给解释器看的（运行时基本不检查），是给读代码的人和 mypy 看的。

## 够用的三件套

```python
from typing import Optional

def greet(name: str, times: int = 1) -> str:
    return ("hi " + name + " ") * times

def find_user(uid: int) -> Optional[dict]:
    return users.get(uid)   # 可能返回 None，Optional 写明了
```

## 容器和结构化数据

```python
from typing import TypedDict

class User(TypedDict):
    name: str
    age: int

def ages(users: list[User]) -> list[int]:   # Python 3.9+ 可直接写 list[int]
    return [u["age"] for u in users]
```

## 我的取舍

- 公共函数、容易传错类型的地方：注解写全，收益最大。
- 内部小脚本：只给函数签名加注解，局部变量不加。
- `Optional[X]` 就是 `X | None`（3.10+ 可直接写 `X | None`）。
- 想真正拦住错误要跑 mypy 或 pyright，只写注解不跑检查等于只写了一半。
