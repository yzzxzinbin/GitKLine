<h1 align="center">
  <span style="color: #d4a853;">📈 Git</span>KLine 
</h1>

<p align="center">
  <img src="https://img.shields.io/badge/GitKLine-v1.0-1a1a2e?style=for-the-badge&logo=git&logoColor=d4a853&labelColor=0c1019&color=d4a853">
  <br>
<img src="https://img.shields.io/badge/Chromium-%3E%3D86-0c1019?style=flat-square&logo=google-chrome&logoColor=d4a853&color=0c1019" alt="Chromium Version">
  <img src="https://img.shields.io/badge/License-MIT-0c1019?style=flat-square&color=0c1019">
  <img src="https://img.shields.io/badge/IndexedDB-Cache-0c1019?style=flat-square&color=0c1019">
</p>
<p align="center">
  <b>将 Git 提交历史可视化为 K 线图</b><br>
  每个代码文件是一支股票，每次提交是一个交易日
</p>
<p align="center">
  <a href="#项目概述">项目概述</a> ·
  <a href="#快速开始">快速开始</a> ·
  <a href="#核心功能">核心功能</a> ·
  <a href="#架构设计">架构设计</a> ·
  <a href="#开发调试">开发调试</a> 
</p>

## 项目概述 

**GitKLine** 是一款运行在浏览器中的单文件HTML页面，本项目受 [KAgent](https://github.com/JStone2934/KAgent) 启发，该项目将Agent对于文件的读写过程展示为K线图。仿照此项目的思路，GitKLine 将 Git 仓库的提交历史转化为类似K 线图的形式进行可视化呈现。通过GitKLine 你可以直观地看到：
 
- 📈 **代码增长趋势** —— 文件行数随时间的涨跌变化
- 📊 **活跃程度** —— 每次提交引入/删除的代码量（成交量）
- 🔍 **Diff 细节** —— 双击任意 K 线柱查看该次提交的详细代码变更

**立即体验**：[在线演示](https://yzzxzinbin.github.io/GitKLine/)

### 映射关系

| 金融概念 | GitKLine 映射 | 说明                          |
| ---- | ----------- | --------------------------- |
| 开盘价  | 提交前文件行数     | 本次 commit 开始时的代码规模          |
| 收盘价  | 提交后文件行数     | 本次 commit 结束时的代码规模          |
| 最高价  | 提交过程中的最大行数  | 对于修改类提交，顺序diff 过程中文件达到的行数峰值 |
| 最低价  | 提交过程中的最小行数  | 对于修改类提交，顺序diff 过程中文件达到的行数谷值 |
| 成交量  | 新增行数 + 删除行数 | 反映本次改动的剧烈程度                 |
| 涨跌幅  | 行数净变化       | 正值表示代码膨胀，负值表示精简             |
| 上市   | 文件首次创建      | 文件第一次出现在仓库中的提交              |
| 退市   | 文件被删除       | 文件在最后一次提交中被移除               |

### 适用场景

- **代码审计**：快速识别哪些文件经历了最多的修改，定位"热点文件"
- **重构评估**：观察重构前后代码规模的变化趋势
- **新人上手**：通过可视化的方式快速了解项目结构和演化历史
- **演示汇报**：在技术分享中提供直观的可视化界面

## 快速开始 

### 启动方式

**方式一：直接打开**

```bash
git clone https://github.com/yzzxzinbin/GitKLine.git
cd GitKLine
# 直接用浏览器打开
```

**方式二：本地 HTTP 服务器**

```bash
# Python 3
python3 -m http.server 8080

# Node.js
npx serve .

# 访问 http://localhost:8080/GitKlinev1.html
```

### 使用步骤

1. 点击顶部 **「打开仓库」** 按钮，在弹出的文件选择器中选择包含 `.git` 目录的文件夹（通常就是项目根目录）
2. 应用在左侧列出所有源代码文件，按提交次数、行数或文件名排序
3. 点击任意文件，右侧 Canvas 区域会渲染该文件的 K 线图
4. 点击播放按钮，观看 K 线图随提交历史动态增长的过程
5. 鼠标悬停在 K 线柱上查看详细信息，双击打开该次提交的 Diff 详情

> ⚠️ **浏览器要求**：必须使用支持 [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API) 的现代浏览器（Chrome 86+ / Edge 86+）。

### 全局控制快捷键

| 按键 | 功能 |
|------|------|
| `Space` | 播放 / 暂停动画 |
| `←` | 上一帧（前一个 Commit） |
| `→` | 下一帧（后一个 Commit） |
| `Home` | 跳转到第一个 Commit |
| `End` | 跳转到最后一个 Commit |
| `Esc` | 关闭设置面板 |

### Diff 弹窗快捷键

| 按键          | 功能                 |
| ----------- | ------------------ |
| `Shift + ↑` | 上一个变更块             |
| `Shift + ↓` | 下一个变更块             |
| `P`         | 上一个变更块             |
| `N`         | 下一个变更块             |
| `K` / `J`   | 上 / 下一个变更块（Vim 风格） |
| `Esc`       | 关闭 Diff 弹窗         |

### 模拟数据模式

如果浏览器不支持FSA目录访问，或只是想快速体验功能，可以点击 **「模拟数据」** 按钮。系统会生成一个包含 9 个文件、360 次提交的虚拟仓库，模拟真实代码演化过程。

### 大仓库处理建议

本HTML页面为偏娱乐性质的demo，请不要试图使用它去加载例如linux kernel这种超大型仓库。虽然已尝试进行优化，但由于能力有限且JavaScript运行效率远不如Native程序，对于超过 10,000 个 Commit 或 1,000 个文件的大型仓库，如果加载体验较差：

1. **关闭后台预加载**：在设置中关闭"后台预加载"，只计算当前查看的文件
2. **降低 Diff 行数限制**：将"Diff 计算最大行数"从 8000 降低到 更低的数值，减少单文件计算时间
3. **等待缓存建立**：首次打开仓库时，等待一会，让后台任务完成一轮缓存写入。

## 核心功能

### K 线图渲染器

基于 HTML5 Canvas 2D API ，支持以下特性：

- **自适应布局**：根据可见 K 线数量自动计算蜡烛图宽度（2px ~ 28px），在数据稀疏时放大显示，数据密集时压缩显示
- **高清渲染**：自动检测设备 DPR（Device Pixel Ratio），使用 `canvas.width = clientWidth * dpr` 保证在 Retina 屏幕上清晰显示
- **移动平均线**：实时计算并绘制 MA5、MA10、MA20 三条移动平均线
- **十字光标**：鼠标移动时显示垂直/水平参考线，并在右侧和底部显示对应的价格和日期标签
- **Tooltip 信息面板**：悬停时显示开盘价、收盘价、最高价、最低价、涨跌幅、成交量、MA 值，以及 Commit 的标题和描述信息

### 动画回放系统

- **播放控制**：支持播放、暂停、逐帧前进/后退
- **速度调节**：8 档速度可选（0.5x、1x、2x、4x、8x、16x、32x、极限），极限模式下使用 `setTimeout(fn, 0)` 实现最快速度
- **进度拖拽**：点击进度条任意位置跳转到对应提交节点
- **边界处理**：播放到末尾自动暂停，Home/End 键快速跳转

### Diff 浏览器

双击 K 线柱打开 Unified Diff 浏览器，提供以下功能：

- **语法高亮**：新增行（绿色）、删除行（红色）、上下文行（灰色），使用 JetBrains Mono 等宽字体
- **Chunk 导航**：支持键盘快捷键在变更块之间快速跳转，当前变更块有金色边框高亮
- **一键复制**：
  - 复制更改前文本（仅保留删除行和上下文行）
  - 复制更改后文本（仅保留新增行和上下文行）
  - 复制标准 Diff 格式（含 `diff --git` 文件头和 `@@` 块头）
- **Commit 信息展示**：在 Diff 顶部显示 Commit 的完整标题、描述、Hash 和日期

### 文件列表管理

- **搜索过滤**：实时按文件名过滤，支持部分匹配
- **多维度排序**：按提交次数（默认）、按最终行数、按文件名排序
- **状态标记**：
  - "NEW" 标签：仅有一次提交的新文件
  - "退市"标签：最终被删除的文件
  - 涨跌颜色：绿色表示最终行数增加，红色表示减少
- **懒加载计算**：列表中的文件不会立即计算 K 线数据，只有滚动到可视区域或点击选中时才会触发计算

### 个性化设置

点击右上角齿轮图标打开设置面板：

| 设置项         | 默认值    | 说明                                          |
| ----------- | ------ | ------------------------------------------- |
| Diff 计算最大行数 | 8000 行 | 超过此限制时，Diff 计算从精确模式降级为快速估算模式（仅统计行数差，不计算上下文） |
| 主题配色        | 深邃黑金   | 提供四种预设主题：深邃黑金、明亮简洁、深海蓝、高对比                  |
| 涨跌配色        | 红涨绿跌   | 红涨绿跌（中国 A 股风格）或 红跌绿涨（国际/美股风格）               |
| 后台预加载       | 开启     | 导入仓库后，自动在后台计算所有文件的 K 线和 Diff 数据             |
| 本地 Diff 缓存  | /      | 查看和清理 IndexedDB 中的缓存数据，按仓库隔离                |

## 架构设计

### 项目结构

```
GitKLine/
├── GitKlinev1.html      # 主应用（单文件，包含全部 HTML/CSS/JS）
├── README.md            # 本文件
└── LICENSE
```

单文件设计是 GitKLine 的最初选择。所有代码内联在一个 HTML 文件中，无需构建工具、无需 npm install、无需打包编译。这意味着：

- 零依赖管理成本
- 零构建配置
- 可以直接通过 `file://` 协议打开
- 易于分享和存档

### 技术选型

GitKLine 采用**零框架**设计，全部依赖原生 Web API 和pako解压库：

- **渲染**：原生 HTML5 Canvas 2D API，无 Chart.js / D3.js 等图表库
- **样式**：纯 CSS3
- **状态管理**：原生 JavaScript 对象
- **Git 解析**：复现浏览器端 Git 解析器
- **解压**：pako.js（zlib inflate 的 JavaScript 实现）
- **存储**：浏览器原生 IndexedDB
- **并发**：Web Worker 线程池

### 核心模块
```bash
GitKLine/
├── 📦 BlockMemoryFS          # 内存文件系统（分层缓存）
│   ├── 小文件 (<100MB) → 全量缓存
│   └── 大文件 (≥100MB) → 10MB 分块 + LRU 淘汰
│
├── 🧵 WorkerPool             # Web Worker 池（4~8 线程）
│   ├── 轻量 Diff（行数统计）
│   └── 完整 Diff（上下文对比）
│
├── 📋 TaskQueue              # 三级任务调度队列
│   ├── priority: 当前选中文件（立即执行）
│   ├── light: 可见区域文件（懒加载）
│   └── full: 后台预加载（空闲时执行）
│
├── 💾 IndexedDBManager       # 持久化缓存管理
│   ├── 仓库隔离（repoId 基于目录名哈希）
│   ├── Commit Hash 版本校验
│   └── 批量写入 + 写入队列去重
│
├── 🔧 GitParser              # 浏览器端 Git 解析器
│   ├── 松散对象（loose objects）读取
│   ├── Pack 文件索引 + 流式解压
│   └── Delta 链解析（REF_DELTA / OFS_DELTA）
│
└── 📈 ChartRender            # Canvas K 线渲染
    ├── 自适应 DPR 高清渲染
    ├── MA5/MA10/MA20 移动平均线
    └── 十字光标 + Tooltip 数据提示
```

### 三级缓存策略

GitKLine 在数据读取路径上设置了三级缓存，层层拦截磁盘 I/O：

| 层级 | 缓存对象 | 容量 | 淘汰策略 | 典型命中率场景 |
|------|----------|------|----------|---------------|
| L1 | 原始文件内容 | 小文件无上限 / 大文件 10 块 | LRU 时间戳 | Pack 文件多次随机读取 |
| L2a | 松散 Git 对象 | 500 条 | Map 顺序 LRU | 同一对象被多个 Commit 引用 |
| L2b | Pack 解压后对象 | 1500 条 | Map 顺序 LRU | Delta 链中的中间对象复用 |
| L3 | Delta 基对象 | 500 个 / 100MB | 双限制 LRU | 同一基对象被多个 Delta 引用 |

## 开发调试

### 启用调试日志

取消注释 `debugLog` 函数中的日志输出：

```javascript
function debugLog(msg) {
    // 开发时取消注释以启用日志
    console.log('[WebGitKLine]', msg);
    // ...
}
```

`debugLog` 函数默认输出到浏览器控制台，同时会更新顶部状态栏显示关键进度。日志分类如下：

| 前缀 | 内容 |
|------|------|
| `[System]` | Worker 池初始化、主题切换、设置保存 |
| `[Repo]` | 仓库解析进度、文件历史分析、K 线计算 |
| `[IDB]` | 数据库初始化、缓存读写、仓库元数据 |
| `[TaskQueue]` | 任务调度、Worker 分配、队列状态 |
| `[Worker]` | Diff 计算完成、数据应用 |
| `[LazyLoad]` | 懒加载触发、IntersectionObserver 回调 |

由于该项目是一个纯粹的 HTML/CSS/JS 单文件页面，部署非常简单：

1.  **静态托管**：将 `GitKLine.html` 文件上传至任何静态 Web 服务器（如 GitHub Pages、Netlify等）。
2.  **本地使用**：直接双击运行，或通过 `npx serve .` 等轻量级工具提供本地服务。

> **注意**：由于浏览器安全策略的限制，`showDirectoryPicker` API 仅在安全上下文（HTTPS 或 localhost）中可用。如需在远程服务器上使用，可能需要通过 HTTPS 访问。

### 常见问题排查

**问题：点击"打开仓库"后无响应**

- 检查浏览器是否支持 File System Access API
- 确保选择的是包含 `.git` 目录的文件夹，而不是 `.git` 目录本身
- 检查浏览器控制台是否有权限错误

**问题：K 线图显示为直线或异常**

- 可能是 Light路径下`MAX_LINES` 配置错误导致 Diff 计算降级，该降级策略是基于对性能的考量，你可以手动修改HTML代码中的数值定义

**问题：Diff 弹窗加载缓慢**

- 首次计算需要读取 Git 对象并解压后做diff运算，大型文件可能需要几秒
- 后续同一 Commit 的 Diff 会从 IndexedDB 缓存读取，速度显著提升

**问题：IndexedDB 空间不足**

- 在设置面板中清理不需要的仓库缓存
- 浏览器通常限制 IndexedDB 为磁盘空间的 50%，大型仓库可能需要清理
- 尝试关闭后台预加载选项减少对IDB的使用

---
## 许可证

[MIT License](LICENSE)
