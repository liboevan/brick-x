[English](README.en.md) | 中文

# Brick X - Docker Compose 编排

本目录包含 Brick X 系统的 Docker Compose 编排配置，专注于多服务的协调和部署。

## 🏗️ 架构

### Docker Compose 职责
- **服务编排** - 网络、依赖关系、健康检查
- **环境管理** - 统一的环境变量和配置
- **服务发现** - 服务间的网络通信
- **健康监控** - 确保服务正确启动顺序

### 服务组成
- **brick-x-auth-service**: 认证服务 (端口 17101)
- **brick-x-webapp**: Web 应用 (端口 17100)
- **brick-x-clock**: 时钟服务 (端口 17103)

## 🚀 快速开始

### 前置条件
确保各个服务已构建：
```bash
cd brick-x-auth-service && ./scripts/build.sh
cd brick-x-webapp && ./scripts/build.sh
cd brick-x-clock && ./scripts/build.sh
```

### 启动所有服务
```bash
cd brick-x-compose
docker-compose up -d
```

### 基本命令
```bash
# 启动所有服务
docker-compose up -d

# 停止所有服务
docker-compose down

# 检查状态
docker-compose ps

# 查看日志
docker-compose logs

# 重启特定服务
docker-compose restart brick-x-auth-service
```

## 📋 文件

### 核心配置
- **`docker-compose.yml`** - 服务定义和编排

## 🔧 Docker Compose 配置

### 网络
- **el-brick-x-network**: 服务通信的桥接网络

### 依赖关系
- Webapp 依赖于 auth 服务健康状态
- Clock 服务独立运行
- 健康检查确保正确的启动顺序

### 健康检查
- Auth 服务: `http://localhost:17101/health`
- Webapp: `http://localhost:17100/`
- Clock 服务: `http://localhost:17103/health`

## 🌐 服务地址

- **认证服务**: http://localhost:17101
- **Web 应用**: http://localhost:17100
- **时钟服务**: http://localhost:17103

## 📊 监控

### 状态命令
```bash
# 显示所有服务
docker-compose ps

# 检查特定服务
docker-compose ps brick-x-auth-service
```

### 日志命令
```bash
# 所有服务日志
docker-compose logs

# 跟随日志
docker-compose logs -f

# 特定服务日志
docker-compose logs brick-x-auth-service
docker-compose logs brick-x-webapp
docker-compose logs brick-x-clock
```

## 🔄 工作流程

### 开发工作流程
1. **构建服务**：
   ```bash
   cd brick-x-auth-service && ./scripts/build.sh
   cd brick-x-webapp && ./scripts/build.sh
   cd brick-x-clock && ./scripts/build.sh
   ```

2. **使用 compose 启动**：
   ```bash
   cd brick-x-compose
   docker-compose up -d
   ```

3. **监控和调试**：
   ```bash
   docker-compose ps
   docker-compose logs
   ```

### 生产工作流程
1. **部署服务** (由各个服务脚本处理)
2. **使用 compose 编排**：
   ```bash
   docker-compose up -d
   ```
3. **监控健康状态**：
   ```bash
   docker-compose ps
   ```

## 🐛 故障排除

### 常见问题

1. **服务无法启动**
   ```bash
   # 检查镜像是否存在
   docker images | grep brick-x
   
   # 检查各个服务状态
   cd brick-x-auth-service && ./scripts/run.sh status
   cd brick-x-webapp && ./scripts/run.sh status
   cd brick-x-clock && ./scripts/run.sh status
   ```

2. **健康检查失败**
   ```bash
   # 检查服务日志
   docker-compose logs
   
   # 手动测试端点
   curl http://localhost:17101/health
   curl http://localhost:17100/
   curl http://localhost:17103/health
   ```

3. **网络问题**
   ```bash
   # 检查网络
   docker network ls | grep el-brick-x-network
   
   # 如需要重新创建
   docker network rm el-brick-x-network
   docker-compose up -d
   ```

### 调试命令
```bash
# 检查 compose 状态
docker-compose ps

# 查看最近日志
docker-compose logs --tail 50

# 测试端点
curl http://localhost:17101/health
curl http://localhost:17100/
curl http://localhost:17103/health

# 检查容器详情
docker inspect el-brick-x-auth-service
docker inspect el-brick-x-webapp
docker inspect el-brick-x-clock
```

## 🎯 最佳实践

1. **先构建服务** - 启动 compose 前始终构建镜像
2. **使用 compose 进行编排** - 让 compose 处理多服务协调
3. **监控健康检查** - 确保服务健康后再继续
4. **查看日志** - 使用 `docker-compose logs` 监控服务状态
5. **备份配置** - 备份 `docker-compose.yml` 文件 