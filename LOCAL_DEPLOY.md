# 本地部署指南 (Local Deployment Guide)

本文档将指导您如何在本地环境中部署 **微舆 (BettaFish)** 系统。您可以选择使用 Docker（推荐）或直接使用源码部署。

## 📋 前置要求

无论选择哪种部署方式，您都需要准备以下基础环境：

1.  **操作系统**: Windows, Linux, 或 MacOS。
2.  **OpenAI 格式 API Key**: 用于驱动系统的大模型（推荐 Kimi, DeepSeek, Gemini 等）。
3.  **数据库**: PostgreSQL (推荐) 或 MySQL。

---

## 🐳 方法一：Docker 部署（推荐）

Docker 部署是最简单快捷的方式，无需手动配置 Python 环境和依赖。

1.  **安装 Docker**: 确保您的机器上安装了 Docker Desktop (Windows/Mac) 或 Docker Engine (Linux)。
2.  **配置文件**:
    *   在项目根目录下，将 `.env.example` 复制为 `.env`。
    *   修改 `.env` 文件，填入您的 API Key 和数据库配置。
3.  **启动服务**:
    在项目根目录运行：
    ```bash
    docker compose up -d
    ```
4.  **访问**:
    浏览器访问 `http://localhost:5000`。

---

## 🔧 方法二：源码部署

如果您需要进行二次开发或无法使用 Docker，请按照以下步骤进行源码部署。

### 1. 环境准备

*   **Python**: 建议使用 Python 3.9 及以上版本 (推荐 3.11)。
*   **Git**: 用于拉取代码。
*   **数据库服务**: 确保本地安装并启动了 PostgreSQL 或 MySQL 服务，并创建了一个空的数据库（例如命名为 `bettafish`）。

### 2. 获取代码

```bash
git clone https://github.com/666ghj/BettaFish.git
cd BettaFish
```

### 3. 创建 Python 虚拟环境

推荐使用 Conda 或 Python venv 来隔离环境。

**使用 Conda:**
```bash
conda create -n bettafish python=3.11
conda activate bettafish
```

**使用 venv:**
```bash
python -m venv venv
# Windows
venv\Scripts\activate
# Linux/Mac
source venv/bin/activate
```

### 4. 安装依赖

安装项目所需的 Python 依赖包：

```bash
pip install -r requirements.txt
```

> **注意**: 如果不需要本地情感分析模型（CPU版本 Torch），可以编辑 `requirements.txt` 注释掉相关部分。
> 如果导出 PDF 功能报错，请参考 `static/Partial README for PDF Exporting/README.md` 安装系统级依赖 (如 GTK3)。

安装 Playwright 浏览器驱动（用于爬虫）：

```bash
playwright install chromium
```

### 5. 配置环境变量

将 `.env.example` 复制为 `.env`：

**Windows:**
```cmd
copy .env.example .env
```

**Linux/Mac:**
```bash
cp .env.example .env
```

**编辑 `.env` 文件**：
使用文本编辑器打开 `.env`，重点配置以下内容：
*   **DB_***: 数据库连接信息 (HOST, PORT, USER, PASSWORD, NAME)。
*   **LLM 配置**: 填入各个 Agent 的 API Key 和 Base URL (如 `INSIGHT_ENGINE_API_KEY` 等)。

### 6. 初始化

在首次运行前，确保数据库配置正确。应用启动时会自动检查数据库连接。

如果您需要初始化爬虫模块的数据库表，可以运行：

```bash
cd MindSpider
python main.py --init-db
cd ..
```

### 7. 启动系统

在项目根目录下运行主程序：

```bash
python app.py
```

终端显示 `Flask服务器已启动，访问地址: http://0.0.0.0:5000` 即表示启动成功。

访问 `http://localhost:5000` 开始使用。

---

## ❓ 常见问题

1.  **依赖安装慢**: 可以尝试更换 pip 源，例如：
    `pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple`
2.  **数据库连接失败**: 请检查 `.env` 中的 `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD` 是否正确，以及数据库服务是否已启动。
3.  **PDF 导出失败**: 确保安装了 `WeasyPrint` 所需的系统库（Windows 下通常需要安装 GTK3 Runtime）。

如有更多问题，请查阅 `README.md` 或提交 Issue。
