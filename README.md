# Claude Code With Codex 扩展插件

## 🚀 快速安装

### 第一步：安装本插件

```bash
# 添加 Blyrin 插件市场源
/plugin marketplace add https://github.com/Blyrin/cc-with-codex

# 下载并安装 c2 插件
/plugin install c2@cc-with-codex
```

### 第二步：配置 Codex MCP (重要)

为了让 Claude Code 能够与 Codex 协作，你需要安装 Codex MCP <https://github.com/GuDaStudio/codexmcp>。

**前置要求**：请确保已安装 `uv` 工具。
- Windows: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
- Mac/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`

**安装命令**：

```bash
# 1. 移除旧版 codex (如果有)
claude mcp remove codex

# 2. 安装 Codex MCP
claude mcp add codex -s user --transport stdio -- uvx --from git+https://github.com/GuDaStudio/codexmcp.git codexmcp
```

### 第三步：设置系统提示词

请将本仓库中 `CLAUDE.example.md` 文件的所有内容复制到你的 `~/.claude/CLAUDE.md` 文件中。

## 💡 使用指南

安装完成后，你可以通过以下方式使用：

1. **深度任务分析 (Scout Agent)**
   强制使用 Scout Agent 来进行更紧密的上下文分析：
   ```bash
   /c2:scout <你的任务描述>
   ```

### 插件更新

保持插件为最新版本：
```bash
/plugin marketplace update https://github.com/Blyrin/cc-with-codex
```

## ✨ 核心特性

### 🤖 智能代理系统

- **`worker` (执行者)**: 负责具体的行动计划执行，如代码修改或命令运行。
- **`scout` (侦查员)**: 深入分析代码库结构与逻辑，生成详细的调查报告。
- **`recorder` (记录员)**: 专注于生成和维护高质量的项目技术文档。

### 📝 文档驱动开发

- **`/c2:initDoc`**: 快速初始化项目的核心文档结构。
- **`/c2:what`**: 智能编程助手，提供清晰的技术指导与建议。

### 🔧 高级工作流

- **`/c2:scout`**: 采用"先侦查，后行动"的策略，专门处理复杂的开发任务。
- **`/c2:review`**: 专门的代码审查代理，用于在提交前进行安全和质量检查。

## 📖 推荐工作流程

### 1. 项目初始化
首次接入时，建立完整的文档体系：
```bash
/c2:initDoc
```

### 2. 日常开发
获取编程思路或进行架构分析：
```bash
# 获取实现思路
/c2:what "如何实现用户登录功能"

# 深度代码架构分析
/c2:scout "分析当前鉴权逻辑，寻找最佳扩展点"

# 代码审查
/c2:review
```
