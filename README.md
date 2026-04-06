# FullTXTLocal - 本地文件全文搜索引擎

<p align="center">
  <strong>快速、精准的本地文件全文搜索工具</strong>
</p>

<p align="center">
  <a href="#-功能特性">功能特性</a> •
  <a href="#-快速开始">快速开始</a> •
  <a href="#-支持格式">支持格式</a> •
  <a href="#-使用指南">使用指南</a> •
  <a href="#-技术栈">技术栈</a> •
  <a href="#-项目结构">项目结构</a>
</p>

---

## 📖 项目简介

**FullTXTLocal** 是一款基于 Go + Wails 开发的本地文件全文搜索引擎桌面应用。类似 FullTXTLocalSearcher，可帮助您在电脑上的所有文件中快速搜索包含特定关键词的内容。

### 核心优势

- ✅ **原生桌面应用**：基于 Wails 2 构建，提供原生 Windows 体验
- ✅ **全文索引**：基于 Bleve 2 的高效全文搜索引擎
- ✅ **实时监控**：文件变更自动更新索引，无需手动干预
- ✅ **多格式支持**：支持 Office 文档、PDF、代码文件等 30+ 种格式
- ✅ **轻量快速**：启动迅速，搜索响应毫秒级
- ✅ **系统托盘**：后台静默运行，不占用任务栏空间

---

## ✨ 功能特性

### 🔍 搜索功能
- **高级搜索**：智能分词，支持部分匹配（默认模式）
- **精确匹配**：完整短语匹配，词语顺序不可颠倒
- **正则搜索**：支持正则表达式，满足高级搜索需求
- **文件名权重**：文件名匹配权重是内容的 2 倍，优先显示相关文件

### 📂 索引管理
- **自动索引**：添加目录后自动扫描并建立索引
- **增量更新**：文件变化自动触发索引更新（500ms 防抖）
- **目录管理**：支持添加/移除多个索引目录
- **状态监控**：实时显示索引进度和状态

### 💻 用户体验
- **文件预览**：点击搜索结果即可查看文件内容预览
- **快速打开**：使用系统默认程序一键打开文件
- **路径复制**：点击即可复制文件路径到剪贴板
- **系统托盘**：右键菜单快速访问，开机自启支持

---

## 🚀 快速开始

### 方式一：直接运行（推荐）

1. **下载可执行文件**：`FullTXTLocal-v1.1.2.exe`（约 22MB）
2. **双击运行**：启动后自动打开主界面
3. **添加索引目录**：进入"索引目录管理"添加需要索引的文件夹
4. **开始搜索**：等待索引完成后即可搜索

### 方式二：从源码构建

#### 前置要求

- **Go** >= 1.25：https://go.dev/dl/
- **Node.js** >= 16：https://nodejs.org/
- **Wails CLI** >= 2.12：`go install github.com/wailsapp/wails/v2/cmd/wails@latest`

#### 构建步骤

```bash
# 1. 克隆项目
git clone <repository-url>
cd fulltextlocal-go

# 2. 安装前端依赖
cd frontend
npm install
cd ..

# 3. 安装 Go 依赖
cd backend
go mod tidy
cd ..

# 4. 构建应用
.\build.bat
# 或手动构建
cd backend
wails build -platform windows/amd64
```

#### 构建产物

编译后的文件位于：`build\bin\FullTXTLocal.exe`

#### 开发模式

```bash
cd backend
wails dev
```

开发模式特性：
- ✅ 自动打开窗口
- ✅ 前端修改自动热重载
- ✅ Go 代码修改自动重启
- ✅ 支持浏览器 DevTools（F12 调试）

---

## 📂 支持格式

| 类型 | 格式 |
|------|------|
| **Office 文档** | .docx .xlsx .pptx |
| **PDF** | .pdf |
| **文本文件** | .txt .md .csv .log .ini .yaml .yml .json .xml .toml .cfg .conf |
| **代码文件** | .go .py .js .ts .java .c .cpp .h .rs .php .rb .sh .sql .bat .ps1 |
| **Web 文件** | .html .htm .css |

> 💡 文件大小限制：超过 10MB 的文件只读取前 10MB 内容

---

## 📖 使用指南

### 1️⃣ 添加索引目录

**方法 1：通过界面**
1. 点击底部 **「索引目录管理」** → **「展开」**
2. 粘贴文件夹路径或点击 **「选择文件夹」** 按钮
3. 点击 **「添加」**，程序会自动扫描并建立索引

**方法 2：通过 API**
```bash
curl -X POST http://localhost:9922/api/dirs ^
  -H "Content-Type: application/json" ^
  -d "{\"path\":\"D:\\Documents\"}"
```

### 2️⃣ 搜索文件

1. 在顶部搜索框输入关键词
2. 选择搜索模式：
   - **高级搜索**（默认）：智能分词，适合日常使用
   - **精确匹配**：完整短语匹配
   - **正则搜索**：支持正则表达式
3. 按回车或点击 **「搜索」** 按钮

### 3️⃣ 查看结果

- **点击结果项**：右侧显示文件预览
- **点击「打开」**：使用系统默认程序打开文件
- **点击「复制」**：复制文件路径到剪贴板

### 4️⃣ 索引管理

- **查看状态**：展开「索引目录管理」查看总文件数和已索引数量
- **移除目录**：点击目录右侧的 **「移除」** 按钮
- **重建索引**：在目录管理区域点击重建按钮

---

## 🔧 高级配置

### 自定义索引存储路径

默认索引存储在：`%USERPROFILE%\.fulltextlocal-go\bleve.db`

**指定到其他位置**（如 D 盘）:
```bash
.\FullTXTLocal.exe -i D:\anytxt-index
```

### 开机自启动

**方法 1：放入启动文件夹**
1. 按 `Win + R`，输入 `shell:startup`，回车
2. 创建 `FullTXTLocal.exe` 的快捷方式
3. 右键快捷方式 → 属性 → 目标，添加参数

**方法 2：使用任务计划程序**
1. 打开"任务计划程序"
2. 创建基本任务 → 名称："FullTXTLocal"
3. 触发器："当前用户登录时"
4. 操作："启动程序" → 选择 `FullTXTLocal.exe`

### 索引数据清理

删除 `%USERPROFILE%\.fulltextlocal-go\` 文件夹即可重置所有索引。

---

## 💡 使用技巧

### 技巧 1：组合搜索
使用空格分隔多个关键词：
```
观音山 花名册
```
→ 同时包含"观音山"和"花名册"的文档

### 技巧 2：利用文件名权重
文件名匹配权重是内容的 2 倍：
- 搜索"报告" → 优先显示文件名包含"报告"的文档
- 搜索"2024 总结" → 优先显示文件名包含这些词的文档

### 技巧 3：正则表达式示例

| 模式 | 说明 | 示例 |
|------|------|------|
| `你好.*世界` | 匹配"你好"和"世界"之间的内容 | 你好，美丽的世界 |
| `\d{4}-\d{2}-\d{2}` | 匹配日期格式 YYYY-MM-DD | 2024-01-15 |
| `\d+` | 匹配一个或多个数字 | 12345 |
| `[A-Za-z]+` | 匹配英文字母 | Hello |

---

## 🛠️ 技术栈

### 后端技术

| 模块 | 使用库 | 版本 |
|------|--------|------|
| **桌面框架** | [Wails v2](https://wails.io/) | 2.12.0 |
| **全文索引** | [Bleve v2](https://github.com/blevesearch/bleve) | 2.5.7 |
| **文件监听** | [fsnotify](https://github.com/fsnotify/fsnotify) | 1.9.0 |
| **PDF 解析** | [ledongthuc/pdf](https://github.com/ledongthuc/pdf) | latest |
| **Excel 解析** | [excelize v2](https://github.com/xuri/excelize) | 2.10.1 |
| **系统托盘** | [getlantern/systray](https://github.com/getlantern/systray) | 1.2.2 |
| **Word/PPT** | 标准库 archive/zip + XML 解析 | - |

### 前端技术

| 模块 | 使用库 | 版本 |
|------|--------|------|
| **框架** | [Vue 3](https://vuejs.org/) | 3.4.0 |
| **构建工具** | [Vite](https://vitejs.dev/) | 5.0.0 |
| **通信** | Wails Runtime | 自动注入 |

### 开发语言

- **后端**：Go 1.25
- **前端**：JavaScript (Vue 3 SFC)

---

## 📁 项目结构

```
fulltextlocal-go/
├── backend/                    # 后端 Go 代码
│   ├── main_wails.go           # Wails 桌面应用主入口
│   ├── main_web.go             # Web 服务器入口（可选）
│   ├── app.go                  # 应用核心逻辑（Wails 服务）
│   ├── app_lifecycle.go        # 生命周期管理
│   ├── tray.go                 # 系统托盘
│   ├── utils_windows.go        # Windows 工具函数
│   ├── wails.json              # Wails 配置文件
│   ├── go.mod                  # Go 模块定义
│   ├── api/                    # HTTP API（Web 模式使用）
│   │   └── handler.go          # HTTP 路由与处理
│   ├── internal/               # 内部包
│   │   ├── indexer/            # Bleve 全文索引引擎
│   │   │   └── indexer.go
│   │   ├── parser/             # 文件解析器
│   │   │   └── parser.go
│   │   └── watcher/            # 文件变更监听
│   │       └── watcher.go
│   └── build/                  # 构建输出
│       └── bin/
│           └── FullTXTLocal.exe
├── frontend/                   # 前端 Vue 3 代码
│   ├── src/
│   │   ├── main.js             # 前端入口
│   │   ├── App.vue             # 主应用组件
│   │   └── components/         # Vue 组件
│   │       ├── Header.vue          # 应用头部（含帮助按钮）
│   │       ├── SearchSection.vue   # 搜索区域
│   │       ├── ResultsSection.vue  # 结果展示区
│   │       ├── DirectoryManager.vue # 目录管理
│   │       ├── AboutModal.vue      # 关于/帮助对话框
│   │       └── DonateModal.vue     # 捐赠弹窗
│   ├── dist/                   # 构建产物（嵌入到 exe）
│   ├── public/                 # 静态资源
│   │   └── about.html          # 帮助页面内容
│   ├── package.json            # npm 依赖配置
│   └── vite.config.js          # Vite 配置
├── build.bat                   # 一键构建脚本
├── cleanup-port.ps1            # 端口清理脚本
├── README.md                   # 项目说明（本文件）
├── 使用说明.md                 # 用户使用手册
├── 使用指南.md                 # 技术使用指南
├── PROJECT_STRUCTURE.md        # 项目结构说明
└── QUICKSTART.md               # 快速开始指南
```

---

## ❓ 常见问题

### Q: 搜索不到结果？
**A**: 请确认：
1. 已添加包含该关键词的文件夹到索引
2. 索引状态显示"就绪"
3. 尝试更换搜索模式（高级/精确/正则）

### Q: 索引速度很慢？
**A**: 首次索引大量文件时需要较长时间，这是正常的。后续会自动增量更新，速度会快很多。

### Q: 端口被占用？
**A**: 检查是否已运行 FullTXTLocal（查看系统托盘），或关闭占用 9922 端口的其他进程。

### Q: 如何查看日志？
**A**: 查看项目目录下的日志文件：
- `error.log` - 错误日志
- `output.log` - 输出日志
- `run.log` - 运行日志

### Q: 支持 Mac 或 Linux 吗？
**A**: 当前版本仅支持 Windows 10/11。如需跨平台支持，请查看 Wails 文档。

### Q: PDF 文件无法预览？
**A**: FullTXTLocal 仅支持文本型 PDF。扫描版 PDF（图片格式）无法提取文本内容，请使用 PDF 阅读器查看。

---

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 提交 Pull Request

---

## 📄 许可证

本项目采用 MIT 许可证 - 查看 LICENSE 文件了解详情

---

## 📞 联系方式

- **作者**：JasonMetal
- **产品版本**：1.1.2
- **公司**：FullTXTLocal

---

## ⭐ 支持项目

如果您觉得这个项目有用，请给一个 Star ⭐，这将是对我们最大的支持！

---

**祝您使用愉快！** 🎉
