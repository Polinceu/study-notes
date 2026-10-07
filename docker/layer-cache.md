# Docker：层缓存，COPY 顺序有讲究

## 原理：一层变了，后面全重建

Dockerfile 每一行是一个层，某一层变了，它之后的所有层缓存失效。
所以：**变化慢的放前面，变化快的放后面**。

## 反面 vs 正面（Node 项目）

```dockerfile
# 反面：改一行代码，npm ci 重跑 3 分钟
COPY . .
RUN npm ci
RUN npm run build
```

```dockerfile
# 正面：依赖层单独缓存，改代码只重建后面
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
```

`package.json` 没变，`npm ci` 那层直接命中缓存，构建秒级。

## 调试缓存

```bash
docker build --no-cache -t myapp .   # 强制全重建
docker build --progress=plain .      # 看每层 CACHED 还是 DONE
```

## 进阶：挂载缓存（BuildKit）

```dockerfile
# syntax=docker/dockerfile:1
RUN --mount=type=cache,target=/root/.npm npm ci
```

依赖下载缓存挂在宿主机，多次构建复用，
CI 上效果特别明显。

一句话：把 Dockerfile 当代码 review，
COPY 的顺序就是省时间的顺序。
