# Linux：chmod 权限，一张表记下来

```
r=4  w=2  x=1
755 = rwxr-xr-x（所有者读写执行，其他人读执行）
644 = rw-r--r--（所有者读写，其他人只读）
600 = rw-------（只有所有者能读写）
700 = rwx------（只有所有者能进）
```

```bash
chmod 755 script.sh     # 脚本可执行
chmod 600 ~/.ssh/id_rsa # 私钥只能自己读
chmod -R 755 dir/        # 递归改
chown user:group file    # 改所有者
```

- 私钥 `600` 是硬性要求，权限太开放 ssh 直接拒绝用。
- 目录需要 `x` 才能 `cd` 进去，只给 `r` 是进不去的。
- `umask` 决定新建文件的默认权限，`umask 022` 对应 644/755。
