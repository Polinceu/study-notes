# Docker：volume，容器删了数据还在

容器里的文件默认跟着容器走：`docker rm` 一删，数据库文件就没了，
volume 就是把数据放到容器外面。

## 两种挂法

```bash
docker run -v mydata:/var/lib/mysql mysql    # 具名 volume，Docker 管理位置
docker run -v /home/me/data:/data nginx      # bind mount，直接挂宿主机目录
```

## 常用管理命令

```bash
docker volume ls              # 看有哪些 volume
docker volume inspect mydata  # 看它实际存在宿主机哪个路径
docker volume rm mydata       # 删 volume（数据一起删，确认好再执行）
```

## 选哪个

- 数据库、需要跨容器共享的数据：具名 volume，备份迁移方便。
- 配置文件、开发时想直接改宿主机文件：bind mount。
- 注意：bind mount 挂一个不存在的宿主机目录，Docker 会自动建一个空目录，
  路径敲错了就等于挂了个空目录进去，启动不报错但数据是空的。
