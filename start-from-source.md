# 从源码启动 Crush

本文介绍如何从源码拉取、编译并启动 Crush。Crush 是一个基于 Go 的
终端 AI 编程助手（TUI 应用），仓库地址为
`github.com/charmbracelet/crush`（本项目为其 fork）。

## 环境要求

- **Go 1.27 或更高版本**（`go.mod` 中声明的版本为 `go 1.27.0`）。
  可用 `go version` 检查本机版本。
- 由于依赖较多，首次编译需要联网下载 Go modules。
- 在 Linux/macOS 上编译时建议使用
  `CGO_ENABLED=0`（Go >= 1.27 还常配合 `GOEXPERIMENT=greenteagc` 使用）。

## 获取源码

```bash
git clone https://github.com/<your-fork>/crush.git
cd crush
```

## 编译与启动

### 1. 直接运行（开发模式）

```bash
go run . --help     # 查看可用命令
go run .            # 启动交互式 TUI
```

### 2. 编译成二进制后运行

```bash
# 普通编译
go build .

# 或 Linux/macOS 下禁用 CGO 编译
CGO_ENABLED=0 go build .

# 运行
./crush            # 或 Windows 下 .\crush.exe
```

`go run .` / `go build .` 会在第一次构建时自动下载全部依赖并缓存，
耗时取决于网络状况。

## 首次使用前的配置

Crush 需要连接模型提供商（Anthropic、OpenAI、Gemini 等）才能工作，
首次运行前需登录：

```bash
go run . login     # 按提示选择平台并完成授权
go run . models    # 查看当前可用的模型
```

配置写入后存放在 crush 的配置目录（`go run . dirs` 可查看具体的
配置与数据目录）。配置格式支持 `crushrc`（Bash 格式，推荐）与
`crush.json`。

## 常用用法

```bash
crush                                     # 交互式模式（TUI）
crush run "解释一下这个仓库的架构"          # 非交互单次提问
crush --debug --cwd /path/to/project      # 带调试日志、指定工作目录
crush --continue                          # 继续最近一次会话
crush --session <session-id>              # 继续指定会话
crush --yolo                              # 自动同意所有权限（慎用）
```

## 常见问题

- **没有配置提供商/未登录**：先执行 `crush login`。
- **编译很慢**：首次构建需下载并编译大量依赖，属正常现象，后续会命中缓存。
- **提示缺少 CGO / 编译报错**：Linux/macOS 下用
  `CGO_ENABLED=0 go build .` 重试。
