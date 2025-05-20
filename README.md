# NockChain Docker 使用教程

- ✈️ t.me/Web3um
- 推特: https://x.com/m7ricks/

本教程将指导您如何使用 Docker Compose 来部署和管理 NockChain 节点。
![image](https://github.com/user-attachments/assets/a5cc0e60-5a14-4c58-8d44-493fad895e32)


## 前提条件

docker、docker-compose

## 项目结构

```
nockchain/
├── README.md                # 本文档
├── docker-compose.yml       # Docker Compose 配置文件
├── Dockerfile               # Docker 镜像构建文件
├── docker-entrypoint.sh     # 容器入口脚本
└── data/                    # 持久化数据目录
```

## 快速开始

### 1. 构建镜像并启动容器

```bash
# 克隆仓库（如果尚未克隆）
git clone https://github.com/alongdlut/nockchain
cd nockchain

# 构建并启动容器
docker-compose up -d
```

### 2. 生成钱包

```bash
docker-compose run nockchain genWallet
```

生成的钱包信息会保存在 `./data` 目录中。

### 3. 运行 Leader 节点

```bash
docker-compose run -d nockchain leader
```

### 4. 运行 Follower 节点

```bash
docker-compose run -d nockchain follower
```

## 常用命令

### 查看容器状态

```bash
docker-compose ps
```

### 查看日志

```bash
docker-compose logs -f
```

### 停止容器

```bash
docker-compose down
```

## 配置说明

### 资源限制

在 `docker-compose.yml` 中，您可以根据自己的机器性能调整资源限制：

```yaml
deploy:
  resources:
    limits:
      cpus: '6.0'      # 限制使用 6 个 CPU 核心
      memory: 16G      # 限制使用 16GB 内存
```

### 网络配置

NockChain 使用自定义 bridge 网络，提供容器间的网络隔离：

```yaml
networks:
  nockchain-net:
    driver: bridge
```

### DNS 配置

为确保容器可以正常解析域名，配置了以下 DNS 服务器：

```yaml
dns:
  - 8.8.8.8
  - 114.114.114.114
```

### 数据持久化

所有的数据都会保存在宿主机的 `./data` 目录中：

```yaml
volumes:
  - ./data:/app/data
```

### 环境变量

可以在 `docker-compose.yml` 中设置以下环境变量：

```yaml
environment:
  - RUST_LOG=info            # 日志级别
  - MINIMAL_LOG_FORMAT=true  # 使用最小化日志格式
```

## 高级用法

### 自定义命令

您可以通过以下方式运行自定义命令：

```bash
docker-compose run nockchain [command]
```

可用命令包括：
- `leader` - 启动 leader 节点
- `follower` - 启动 follower 节点
- `genWallet` - 生成新钱包
- `setKey <公钥>` - 设置挖矿公钥
- `info` - 显示配置信息
- `bash` - 启动终端
- `help` - 显示帮助信息

### 修改入口点

入口点脚本 `docker-entrypoint.sh` 负责处理命令并设置环境。如需自定义行为，可以修改此脚本。

## 故障排除

### 容器无法启动

检查日志以获取详细错误信息：

```bash
docker-compose logs
```

### 网络连接问题

确保 Docker 网络配置正确：

```bash
docker network ls
docker network inspect nockchain-net
```

## 性能优化

- 增加资源限制以提高挖矿性能
- 使用 SSD 存储以提高数据读写速度
- 确保网络连接稳定以维持与其他节点的通信

---

更多信息请参考官方文档或联系开发团队。
