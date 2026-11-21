# Git：log 和 blame，出问题先问"谁改的、什么时候"

代码出问题，先定位是哪次提交引入的，再去看那次提交的上下文，比盯着代码猜快得多。

## 常用组合

```bash
git log --oneline --graph -20        # 最近 20 条，带分支图
git log -S "函数名" --oneline         # 哪次提交增删了这段文字
git log --follow -- 文件路径           # 跟踪一个文件的全部历史（改名也跟）
git blame 文件路径                    # 每一行是谁、哪次提交写的
```

## 找到可疑提交之后

```bash
git show <commit>                    # 看那次提交改了什么
git show <commit>:<文件路径>          # 看那个版本的文件内容
```

## 两个小习惯

- `git log` 默认按时间倒序，`--since="2 weeks ago"` 可以只看近两周。
- `blame` 只能看到"最后一次改这行的人"，想看这行更早的历史，
  对那次提交再 `git log -L 10,20:文件路径` 看行级演变。
