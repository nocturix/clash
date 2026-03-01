# CLAUDE.md

本文件为 Claude Code（claude.ai/code）在处理本仓库代码时提供指导说明。

## 项目概述

Clash 是一个用 Go 语言编写的基于规则的隧道代理。核心功能是通过规则路由实现网络流量的智能转发，支持多种入站和出站协议。

## 常用命令

### 构建项目
```bash
# 构建当前平台
make -j $(go run ./test/main.go) all

# 构建特定平台
make linux-amd64
make darwin-arm64
make windows-amd64

# 构建所有平台
make all-arch
```

### 测试
```bash
# 运行所有测试
go test ./...

# 运行特定测试
go test -v ./adapter/...
go test -v ./dns/...

# 测试配置文件
./clash -t

# 性能测试
cd test && make benchmark
```

### 代码质量
```bash
# 跨平台 lint 检查
make lint

# 自动修复 lint 问题
make lint-fix

# 仅在特定平台 lint
GOOS=linux golangci-lint run ./...
GOOS=darwin golangci-lint run ./...
```

### 运行
```bash
# 使用默认配置运行
./clash

# 指定配置目录
./clash -d /path/to/config/dir

# 指定配置文件
./clash -f /path/to/config.yaml

# 查看版本
./clash -v
```

## 项目架构

### 核心组件

**tunnel/tunnel.go** - 隧道核心
- 管理所有 TCP 和 UDP 连接
- 实现规则匹配和流量路由
- 支持三种模式：Global（全局）、Rule（规则）、Direct（直连）
- 使用 channel 实现连接队列：`tcpQueue` 和 `udpQueue`

**adapter/** - 适配器层
- `adapter/adapter.go`: 代理适配器包装器，处理健康检查和延迟历史
- `adapter/outbound/`: 各种出站协议实现（Shadowsocks、VMess、Trojan 等）
- `adapter/outboundgroup/`: 代理组实现（负载均衡、自动故障转移等）
- `adapter/provider/`: 代理提供者，支持动态加载远程代理列表
- `adapter/inbound/`: 入站连接适配器

**config/** - 配置解析
- `config/config.go`: 配置结构定义
- `config/initial.go`: 配置初始化
- 使用 YAML 格式，通过 yaml.v3 解析

**hub/executor/** - 配置执行器
- 负责将解析的配置应用到各个组件
- `ApplyConfig()` 方法分派配置更新到所有子系统

**listener/** - 入站监听器
- `http/`, `socks/`, `mixed/`: 各协议监听器
- `redir/`, `tproxy/`: 透明代理支持
- `tunnel/`: TUN 设备支持
- `auth/`: 认证中间件

**dns/** - DNS 处理
- `dns/resolver.go`: DNS 解析器
- `dns/enhancer.go`: DNS 增强模式（Fake-IP、redir-host）
- `dns/middleware.go`: DNS 中间件

**rule/** - 规则引擎
- 实现各种规则类型：DOMAIN、IP-CIDR、GEOIP、PROCESS 等
- `rule/parser.go`: 规则解析器

**component/** - 基础组件
- `dialer/`: 网络拨号器
- `fakeip/`: Fake-IP 池管理
- `nat/`: NAT 表，用于 UDP 会话管理
- `process/`: 进程查找器
- `resolver/`: DNS 解析器组件
- `iface/`: 网络接口管理

**constant/** - 常量和接口定义
- 定义了所有核心接口（Proxy、Rule、Conn 等）
- `constant/metadata.go`: 元数据结构
- `constant/context.go`: 连接上下文

### 数据流

1. **入站**: `listener/` 接收连接 → 创建 `ConnContext` → 发送到 `tunnel` 的队列
2. **处理**: `tunnel/process()` 从队列取出连接 → `preHandleMetadata()` 预处理 → `resolveMetadata()` 匹配规则
3. **出站**: 通过匹配的代理（`adapter/outbound/`）建立连接 → `statistic/` 统计 → 双向转发

### 配置热更新

通过 SIGHUP 信号触发配置重新加载，流程：
1. 接收 SIGHUP 信号
2. 调用 `executor.ParseWithPath()` 重新解析配置
3. 调用 `executor.ApplyConfig()` 应用新配置

## Go 版本和工具

- Go 版本: 1.21
- 使用 `golangci-lint` 进行代码检查，配置见 `.golangci.yaml`
- 导入顺序: 标准库 → `github.com/Dreamacro/clash` → 第三方库

## 重要约定

1. **配置结构**: 所有配置相关的结构定义在 `config/config.go`
2. **常量路径**: 使用 `C.Path` 访问配置文件路径
3. **日志**: 使用 `log` 包，支持不同日志级别
4. **错误处理**: 隧道中连接建立失败会记录日志但不阻塞其他连接
5. **并发**: Tunnel 使用多个 goroutine 处理连接（UDP 默认 4 个 worker）

## 测试说明

- 测试配置在 `test/config/` 目录
- 运行测试时使用 `-p 1` 标志避免并发问题
- CI 在 `.github/workflows/test.yaml` 中定义

## 环境变量

- `CLASH_HOME_DIR`: 配置目录
- `CLASH_CONFIG_FILE`: 配置文件路径
- `CLASH_OVERRIDE_EXTERNAL_UI_DIR`: 覆盖外部 UI 目录
- `CLASH_OVERRIDE_EXTERNAL_CONTROLLER`: 覆盖外部控制器地址
- `CLASH_OVERRIDE_SECRET`: 覆盖 RESTful API 密钥
