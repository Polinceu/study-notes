# Git：reflog，删分支/reset 手滑后的后悔药

## reflog 记录了 HEAD 的每一次移动

```bash
git reflog
# a1b2c3d HEAD@{0}: reset: moving to HEAD~2
# d4e5f6g HEAD@{1}: commit: 重要的功能
# ...
```

`git log` 只看得到"现在"的历史，
`reflog` 看得到"你干过什么"，包括被 reset 掉的提交。

## 场景一：reset --hard 错了

```bash
git reset --hard HEAD~2   # 手滑，多回退了一步
git reflog                # 找到回退前的 HEAD@{1}
git reset --hard HEAD@{1} # 回去
```

## 场景二：分支删错了

```bash
git branch -D feature     # 删错了，提交还在 reflog 里
git reflog                # 找到 feature 最后一次的 hash
git branch feature a1b2c3d # 原地复活
```

## 注意事项

- reflog 默认只保留 90 天，救急趁早。
- `git gc` 激进清理可能清掉，重要分支别只靠 reflog。
- push 到远端的内容，远端还有一份，更保险。

记住这个命令，半夜手滑时能救命。
