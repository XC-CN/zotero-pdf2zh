# Zotero PDF2zh - Zotero PDF 翻译插件

> ⚠️ **声明**：本仓库是 [guaguastandup/zotero-pdf2zh](https://github.com/guaguastandup/zotero-pdf2zh) 的**个人副本**，仅为方便本地部署而上传至 GitHub。**所有功能的原始开发和维护均归原作者 [guaguastandup](https://github.com/guaguastandup) 所有**，请前往原项目获取最新版本和技术支持。

<p align="center">
  <b>在 Zotero 中一键翻译 PDF 文献，支持多种翻译引擎和输出格式</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-v4.0.0-blue" alt="version">
  <img src="https://img.shields.io/badge/python-3.8+-green" alt="python">
  <img src="https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey" alt="platform">
</p>

---

## 📖 简介

**Zotero PDF2zh** 是一款 Zotero 插件，帮助科研工作者在 Zotero 中快速翻译 PDF 文献。项目由 Zotero 插件（`.xpi`）和本地 Python 翻译服务端两部分组成：

- **Zotero 插件**：安装在 Zotero 中，提供右键翻译入口和配置界面
- **Python 服务端**：在本地运行 Flask 服务器，负责调用翻译引擎完成实际翻译工作，并提供 Web 进度监控页面

## ✨ 功能特性

### 🌐 多翻译引擎支持

支持两种核心翻译引擎框架，用户可根据需求自由切换：

| 引擎框架 | 说明 |
|---------|------|
| `pdf2zh` | 经典翻译引擎（v1.x） |
| `pdf2zh_next` | 新一代翻译引擎（v2.x），推荐使用 |

支持的 LLM / 翻译服务商：

| 服务商 | 类型 |
|--------|------|
| OpenAI (GPT-4o-mini 等) | LLM |
| Gemini | LLM |
| DeepSeek | LLM |
| Ollama (本地部署) | LLM |
| SiliconFlow / SiliconFlowFree | LLM |
| 智谱 (Zhipu GLM) | LLM |
| 通义千问 MT (Qwen-MT) | LLM |
| 阿里云百炼 (DashScope) | LLM |
| ModelScope | LLM |
| Azure OpenAI | LLM |
| Groq | LLM |
| Grok | LLM |
| AnythingLLM | LLM |
| Dify | LLM |
| Claude Code | LLM |
| OpenAI Compatible（自定义兼容接口） | LLM |
| DeepL | 机器翻译 |
| Google 翻译 | 机器翻译 |
| Bing 翻译 | 机器翻译 |
| Azure 翻译 | 机器翻译 |
| 腾讯机器翻译 | 机器翻译 |

### 📄 多种输出格式

| 输出格式 | 说明 |
|---------|------|
| `mono` | 纯翻译文件 |
| `dual` | 双语对照文件（上下 TB 或左右 LR 排版） |
| `mono-cut` | 裁剪后的纯翻译文件 |
| `dual-cut` | 裁剪后的双语对照文件 |
| `compare` | 原文与译文左右对照 |
| `crop-compare` | 裁剪后再拼接的对照文件 |

### 🔧 其他特性

- **Web 进度监控**：启动服务后访问 `http://localhost:8890` 查看实时翻译进度、历史记录
- **SSE 实时推送**：通过 Server-Sent Events 技术实时推送翻译进度
- **虚拟环境管理**：自动使用 `conda` 或 `uv` 管理 Python 虚拟环境，隔离依赖
- **自动更新**：启动时自动检查更新，支持从 GitHub / Gitee 源获取最新版本
- **术语表支持**：支持自动提取和自定义术语表，保证专业术语翻译一致性
- **OCR 支持**：对扫描版 PDF 提供 OCR 识别翻译
- **Windows EXE 模式**：支持直接使用 `pdf2zh_next` 的 Windows 可执行文件，无需 Python 环境
- **镜像加速**：为中国大陆用户提供 PyPI 镜像加速下载

## 🚀 快速开始

### 环境要求

- **Python** ≥ 3.8
- **Zotero** 7（推荐）
- 操作系统：Windows / macOS / Linux

### 1. 安装 Zotero 插件

1. 在 [Releases](https://github.com/guaguastandup/zotero-pdf2zh/releases) 页面下载最新的 `.xpi` 文件（如 `zotero-pdf-2-zh-v4.0.0.xpi`）
2. 在 Zotero 中：`工具` → `附加组件` → ⚙️ → `Install Add-on From File...`
3. 选择下载的 `.xpi` 文件安装

### 2. 启动翻译服务端

```bash
# 进入 server 目录
cd server

# 安装依赖
pip install -r requirements.txt

# 启动服务（默认端口 8890）
python server.py
```

服务启动后，浏览器访问 `http://localhost:8890` 即可查看 Web 进度监控页面。

### 3. 配置翻译引擎

在 Zotero 插件设置中配置：
- 选择翻译引擎（`pdf2zh` 或 `pdf2zh_next`）
- 选择翻译服务商并填写 API Key
- 设置源语言和目标语言
- 选择输出格式

也可以直接编辑配置文件：
- `server/config/config.json` — `pdf2zh` 引擎配置（服务商 API Key 等）
- `server/config/config.toml` — `pdf2zh_next` 引擎配置（详细翻译参数）

### 4. 开始翻译

在 Zotero 中右键选中 PDF 文献，选择翻译即可。

## ⚙️ 服务端参数

```bash
python server.py [OPTIONS]
```

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `--port` | `8890` | 服务端口号 |
| `--enable_venv` | `True` | 是否自动管理虚拟环境 |
| `--env_tool` | `conda` | 虚拟环境工具（`conda` / `uv`） |
| `--check_update` | `True` | 启动时是否检查更新 |
| `--update_source` | `gitee` | 更新源（`github` / `gitee`） |
| `--enable_mirror` | `True` | 启用 PyPI 镜像加速（中国大陆用户） |
| `--mirror_source` | `https://mirrors.ustc.edu.cn/pypi/simple` | 自定义 PyPI 镜像源 |
| `--enable_winexe` | `False` | 使用 Windows 可执行文件模式 |
| `--skip_install` | `False` | 跳过虚拟环境中的依赖安装 |
| `--debug` | `False` | 开启调试模式 |

## 📁 项目结构

```
zotero-pdf2zh/
├── zotero-pdf-2-zh-v4.0.0.xpi   # Zotero 插件安装包
└── server/                       # Python 翻译服务端
    ├── server.py                 # 主服务（Flask）
    ├── index.html                # Web 进度监控页面
    ├── favicon.svg               # 网站图标
    ├── requirements.txt          # Python 依赖
    ├── config/                   # 配置文件目录
    │   ├── config.json           # pdf2zh 引擎配置
    │   ├── config.toml           # pdf2zh_next 引擎配置
    │   └── venv.json             # 虚拟环境配置
    ├── utils/                    # 工具模块
    │   ├── auto_update.py        # 自动更新（GitHub/Gitee）
    │   ├── config.py             # 配置解析
    │   ├── config_map.py         # 配置映射
    │   ├── cropper.py            # PDF 裁剪和拼接
    │   ├── execute.py            # 带进度的命令执行
    │   ├── record.py             # 翻译记录
    │   ├── task_manager.py       # 任务管理（进度追踪）
    │   └── venv.py               # 虚拟环境管理
    ├── translated/               # 翻译输出目录
    └── doc/                      # 文档
```

## 🔌 API 端点

| 端点 | 方法 | 说明 |
|------|------|------|
| `/` | GET | Web 进度监控页面 |
| `/health` | GET | 健康检查 |
| `/translate` | POST | 翻译 PDF |
| `/crop` | POST | 裁剪 PDF |
| `/crop-compare` | POST | 裁剪对照 |
| `/compare` | POST | 双语对照 |
| `/translatedFile/<filename>` | GET | 下载翻译文件 |
| `/events` | GET | SSE 实时进度推送 |
| `/api/history` | GET | 翻译历史记录 |
| `/api/config` | GET | 当前服务配置信息 |

## ❓ 常见问题

1. **端口被占用**：使用 `--port` 参数指定其他端口，同时在 Zotero 插件设置中修改对应端口号
2. **虚拟环境安装失败**：检查网络连接，或使用 `--enable_mirror True` 开启镜像加速
3. **翻译失败**：检查 API Key 是否正确配置，查看终端输出的错误日志

## 🔗 相关链接

- **原项目 GitHub**: [https://github.com/guaguastandup/zotero-pdf2zh](https://github.com/guaguastandup/zotero-pdf2zh)（⭐ 请前往原项目获取最新版本和支持）
- **原项目 Gitee（国内镜像）**: [https://gitee.com/guaguastandup/zotero-pdf2zh](https://gitee.com/guaguastandup/zotero-pdf2zh)
- **QQ 群**：请在原项目 GitHub 主页查看最新群号（入群口令：github）

## 📝 更新日志

### v4.0.0

- 新增 Web 进度监控页面（`index.html`），支持实时查看翻译进度
- 新增 SSE（Server-Sent Events）实时推送
- 新增翻译历史记录 API
- 新增服务配置信息 API
- 新增健康检查端点
- 新增自动更新模块，支持 GitHub / Gitee 源
- 新增 Windows 可执行文件模式（`--enable_winexe`）
- 优化虚拟环境管理，支持 `conda` 和 `uv`
- 优化错误处理和日志输出
- 优化插件端项目配置
- 修复部分 bug

## 📄 许可证

本项目仅供学习和研究使用。

---

> 💡 **提示**：翻译期间请保持服务端终端窗口开启，请勿关闭。
