# NetLogAI 快速入门指南

本指南将帮助您快速开始使用 NetLogAI 并了解其核心功能。

## 📋 前提条件

在开始之前，请确保您已安装：

- **Windows 10/11** (64位)
- **Visual Studio 2022** 或 **带CMake Tools的Visual Studio Code**
- **Git for Windows**
- **管理员权限** (某些安装步骤需要)

## 🚀 安装步骤

### 步骤 1: 克隆仓库

```bash
# 克隆 NetLogAI 仓库
git clone https://github.com/leochong/netlogai-core.git
cd netlogai-core

# 初始化子模块（包含开源组件）
git submodule update --init --recursive
```

### 步骤 2: 设置构建环境

```bash
# 安装 vcpkg 依赖项
./vcpkg/vcpkg install nlohmann-json lua curl openssl

# 配置 CMake 构建
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
```

### 步骤 3: 构建项目

```bash
# 以 Release 模式构建
cmake --build build --config Release

# 验证构建成功
./build/Release/netlogai.exe --version
```

## 🎯 首次使用

### 测试基本命令

```bash
# 检查 NetLogAI 状态
./build/Release/netlogai.exe status

# 列出可用解析器
./build/Release/netlogai.exe parser list

# 检查 AI 状态
./build/Release/netlogai.exe ai-status
```

### 启动交互式Shell

```bash
# 以交互模式运行 NetLogAI
./build/Release/netlogai.exe

# 在shell中查看帮助
help

# 探索可用命令
browse
```

## 🤖 AI 设置（可选）

要使用 AI 驱动的分析：

```bash
# 交互式 AI 设置向导
./build/Release/netlogai.exe config ai

# 或设置环境变量
export NETLOGAI_AI_PROVIDER=anthropic
export NETLOGAI_ANTHROPIC_KEY=your-api-key
```

## 📊 示例用例

### 1. 分析日志文件

```bash
# 分析示例日志文件
./build/Release/netlogai.exe analyze sample-logs/cisco-router.log

# 搜索特定模式
./build/Release/netlogai.exe analyze --pattern "BGP" sample-logs/
```

### 2. AI 驱动分析

```bash
# 自然语言提问
./build/Release/netlogai.exe ask "网络有什么问题吗？"

# 解释特定错误
./build/Release/netlogai.exe ask error "%LINEPROTO-5-UPDOWN" --device-type cisco-ios
```

### 3. 设备管理

```bash
# 添加网络设备
./build/Release/netlogai.exe device add Router1 --ip 192.168.1.1 --type cisco-ios

# 检查设备列表
./build/Release/netlogai.exe device list

# 测试设备连接
./build/Release/netlogai.exe device show Router1
```

## 🎮 交互式功能

### 日志查看器控制

- **导航**: `j/k` (上/下), `Space/b` (向下/向上翻页)
- **搜索**: `/` (搜索), `n/N` (下一个/上一个匹配)
- **功能**: `l` (行号), `t` (时间戳)
- **帮助**: `h` 或 `?`

### 文件浏览器

- **导航**: 方向键, `Enter` (打开)
- **选择**: `Space` (切换), `a` (全选)
- **操作**: `r` (刷新), `/` (搜索)

## ⚠️ 常见故障排除

### 构建错误

```bash
# 重新安装 vcpkg 依赖项
./vcpkg/vcpkg remove --outdated
./vcpkg/vcpkg install nlohmann-json lua curl openssl

# 清理构建缓存
rm -rf build/
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
```

### 权限问题

```bash
# 以管理员身份运行命令提示符
# 或安装到用户目录
```

### AI 连接问题

```bash
# 检查 AI 状态
./build/Release/netlogai.exe ai-status

# 测试 AI 配置
./build/Release/netlogai.exe ai-test
```

## 📚 下一步

1. **[用户指南](../user-guide.md)** - 学习高级功能
2. **[命令参考](../command-reference.md)** - 探索所有命令
3. **[配置指南](../configuration.md)** - 高级设置
4. **[插件开发](../plugin-development.md)** - 创建自定义功能

## 🆘 需要帮助？

- 📧 **邮箱**: support@netlogai.com
- 🐛 **问题**: [GitHub Issues](https://github.com/netlogai/netlogai-core/issues)
- 📖 **文档**: [项目Wiki](https://github.com/netlogai/netlogai-core/wiki)

---

恭喜！您现在已准备好使用 NetLogAI。🎉