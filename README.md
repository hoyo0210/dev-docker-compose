# Docker Compose 常用配置集合

这是一个收集常用Docker Compose配置文件的仓库，包含了开发和生产环境中经常使用的服务配置。

## 📋 项目概述

本项目提供了各种常用服务的Docker Compose配置模板，包括：
- **ELK Stack** - 日志收集、存储和可视化
- **Filebeat** - 日志文件收集器
- **MySQL** - 关系型数据库
- **Nacos** - 服务注册与发现中心
- **RabbitMQ** - 消息队列
- **Redis** - 缓存数据库

## 📦 版本信息

| 服务 | 版本 | 说明 |
|------|------|------|
| Elasticsearch | 7.9.3 | 搜索引擎 |
| Logstash | 7.9.3 | 日志处理 |
| Kibana | 7.9.3 | 数据可视化 |
| Filebeat | 7.9.3 | 日志收集器 |
| MySQL | 8.0 | 关系型数据库 |
| Nacos | v2.2.3 | 服务注册中心 |
| RabbitMQ | 3.12-management-alpine | 消息队列 |
| Redis | 7.0-alpine | 缓存数据库 |

## 🚀 快速使用

### 前置要求

- Docker Engine 20.10+
- Docker Compose 2.0+

### 使用方式

1. **克隆项目**
   ```bash
   git clone https://github.com/hoyo0210/dev-docker-compose.git
   cd dev-docker-compose
   ```

2. **选择需要的服务**
   ```bash
   # 启动单个服务
   docker-compose -f elk/docker-compose.yml up -d
   docker-compose -f mysql/docker-compose.yml up -d
   docker-compose -f nacos/docker-compose.yml up -d
   docker-compose -f rabbitmq/docker-compose.yml up -d
   docker-compose -f redis/docker-compose.yml up -d
   docker-compose -f filebeat/docker-compose.yml up -d
   ```

3. **验证服务状态**
   ```bash
   docker ps
   ```

## 📁 配置文件结构

```
dev-docker-compose/
├── elk/                    # ELK 日志系统配置
│   ├── docker-compose.yml  # Elasticsearch + Logstash + Kibana
│   └── logstash.conf       # Logstash 配置文件
├── filebeat/              # 日志收集器配置
│   ├── docker-compose.yml  # Filebeat 容器配置
│   └── filebeat.yml        # Filebeat 配置文件
├── mysql/                 # MySQL 数据库配置
│   └── docker-compose.yml  # MySQL + phpMyAdmin
├── nacos/                 # Nacos 注册中心配置
│   └── docker-compose.yml  # Nacos 服务配置
├── rabbitmq/              # RabbitMQ 消息队列配置
│   └── docker-compose.yml  # RabbitMQ + 管理界面
├── redis/                 # Redis 缓存配置
│   └── docker-compose.yml  # Redis 服务配置
└── README.md
```

## 🔧 服务配置详情

### ELK Stack (`elk/`)
- **Elasticsearch**: `http://localhost:9200`
- **Kibana**: `http://localhost:5601`
- **Logstash**: `localhost:5044`
- **功能**: 集中化日志管理、搜索和可视化

### Filebeat (`filebeat/`)
- **配置**: `filebeat/filebeat.yml`
- **日志路径**: `/logs/blm-saas/*.log`
- **功能**: 收集应用日志并发送到ELK Stack

### MySQL (`mysql/`)
- **端口**: `3306`
- **管理界面**: `http://localhost:9999` (phpMyAdmin)
- **用户名**: `root`
- **密码**: `8j2tfUM3q0TkCGbrx0N9`

### Nacos (`nacos/`)
- **控制台**: `http://localhost:8848`
- **gRPC端口**: `9848`
- **用户名**: `nacos`
- **密码**: `nacos`
- **环境变量**: 需要配置 `.env` 文件

### RabbitMQ (`rabbitmq/`)
- **AMQP端口**: `5672`
- **管理界面**: `http://localhost:15672`
- **用户名**: `admin`
- **密码**: `yourpassword`

### Redis (`redis/`)
- **端口**: `6379`
- **持久化**: AOF模式
- **数据卷**: `redis_data`

## 💾 数据持久化

| 服务 | 数据卷 | 说明 |
|------|--------|------|
| Elasticsearch | `esdata` | 索引数据 |
| MySQL | `./data` | 数据库文件 |
| RabbitMQ | `rabbitmq_data`, `rabbitmq_logs` | 消息数据和日志 |
| Redis | `redis_data` | 缓存数据 |
| Nacos | `nacos_logs` | 日志文件 |
| Filebeat | `filebeat-data` | 注册表文件 |

## 🛠️ 常用操作

### 启动服务

```bash
# 启动单个服务
docker-compose -f elk/docker-compose.yml up -d
docker-compose -f mysql/docker-compose.yml up -d
docker-compose -f nacos/docker-compose.yml up -d
docker-compose -f rabbitmq/docker-compose.yml up -d
docker-compose -f redis/docker-compose.yml up -d
docker-compose -f filebeat/docker-compose.yml up -d
```

### 停止服务

```bash
# 停止单个服务
docker-compose -f elk/docker-compose.yml down
docker-compose -f mysql/docker-compose.yml down
docker-compose -f nacos/docker-compose.yml down
docker-compose -f rabbitmq/docker-compose.yml down
docker-compose -f redis/docker-compose.yml down
docker-compose -f filebeat/docker-compose.yml down
```

### 查看服务状态

```bash
# 检查所有服务状态
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

### 查看服务日志

```bash
# 查看特定服务日志
docker logs elasticsearch
docker logs kibana
docker logs logstash
docker logs filebeat-nacos
docker logs mysql
docker logs nacos
docker logs rabbitmq
docker logs redis
```

## 🔐 安全注意事项

⚠️ **重要**: 本配置仅适用于开发环境，生产环境请：

1. 修改所有默认密码
2. 配置防火墙规则
3. 启用SSL/TLS加密
4. 设置访问控制列表
5. 定期备份数据

## 📊 自定义配置

### 环境变量配置

```bash
# Nacos 环境变量配置
# 创建 nacos/.env 文件
MODE=standalone
SPRING_DATASOURCE_PLATFORM=mysql
MYSQL_SERVICE_HOST=mysql
MYSQL_SERVICE_PORT=3306
MYSQL_SERVICE_DB_NAME=nacos
MYSQL_SERVICE_USER=root
MYSQL_SERVICE_PASSWORD=8j2tfUM3q0TkCGbrx0N9
```

### 修改密码

```bash
# 修改 MySQL 密码
# 编辑 mysql/docker-compose.yml 中的 MYSQL_ROOT_PASSWORD

# 修改 RabbitMQ 密码
# 编辑 rabbitmq/docker-compose.yml 中的 RABBITMQ_DEFAULT_PASS
```

### 调整资源限制

```yaml
# 在 elk/docker-compose.yml 中调整 Elasticsearch 内存
environment:
  - ES_JAVA_OPTS=-Xms512m -Xmx512m  # 根据服务器内存调整
```

### 网络配置

```yaml
# Filebeat 需要连接到多个网络
networks:
  - nacos_default    # 连接到Nacos网络
  - elk_default      # 连接到ELK网络
```

## 🐛 故障排除

### 常见问题

1. **端口冲突**
   ```bash
   # 检查端口占用
   netstat -tulpn | grep :9200
   ```

2. **内存不足**
   ```bash
   # 检查系统资源
   docker stats
   ```

3. **网络连接问题**
   ```bash
   # 检查Docker网络
   docker network ls
   docker network inspect <network-name>
   ```

4. **服务启动顺序**
   ```bash
   # 建议启动顺序
   # 1. 先启动基础服务
   docker-compose -f mysql/docker-compose.yml up -d
   docker-compose -f redis/docker-compose.yml up -d
   
   # 2. 再启动中间件
   docker-compose -f nacos/docker-compose.yml up -d
   docker-compose -f rabbitmq/docker-compose.yml up -d
   
   # 3. 最后启动日志系统
   docker-compose -f elk/docker-compose.yml up -d
   docker-compose -f filebeat/docker-compose.yml up -d
   ```

## 🎯 使用场景

### 开发环境
- 快速搭建本地开发环境
- 测试微服务架构
- 日志收集和分析

### 测试环境
- 集成测试
- 性能测试
- 功能验证

### 学习环境
- Docker Compose 学习
- 微服务架构实践
- 日志系统搭建

## 🤝 贡献指南

欢迎贡献更多常用的Docker Compose配置！

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/new-service`)
3. 添加新的服务配置
4. 提交更改 (`git commit -m 'Add new service configuration'`)
5. 推送到分支 (`git push origin feature/new-service`)
6. 打开 Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 📞 支持

如果您遇到问题或有任何疑问，请：

1. 查看 [Issues](https://github.com/hoyo0210/dev-docker-compose/issues) 页面
2. 创建新的 Issue
3. 联系维护者

## 🔗 相关链接

- [项目地址](https://github.com/hoyo0210/dev-docker-compose)
- [Docker 官方文档](https://docs.docker.com/)
- [Docker Compose 文档](https://docs.docker.com/compose/)

---

**注意**: 这些配置文件仅作为参考模板，请根据实际需求进行调整。
