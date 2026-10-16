# Linux：ss 替代 netstat

netstat 很多系统已经不预装了（net-tools 被 iproute2 取代），
`ss` 更快，输出也更全。

## 查监听端口

```bash
ss -tlnp
# -t TCP，-l listening，-n 数字端口不反解，-p 显示进程
```

输出示例：

```
LISTEN 0  128  0.0.0.0:22  0.0.0.0:*  users:(("sshd",pid=123,fd=3))
```

"端口被谁占了"一问就用它。

## 查连接状态

```bash
ss -tan | grep ESTAB          # 已建立的连接
ss -s                         # 各状态统计，一眼看 TIME_WAIT 多少
ss -tnp dst :443              # 到 443 端口的连接和进程
```

## 和 netstat 的对应关系

| netstat        | ss            |
|----------------|---------------|
| netstat -tlnp  | ss -tlnp      |
| netstat -an    | ss -tan       |
| netstat -s     | ss -s         |
| netstat -r     | ip route      |

习惯把 `netstat -tlnp` 换成 `ss -tlnp` 就行，
参数字母基本通用，肌肉记忆不用重练。
