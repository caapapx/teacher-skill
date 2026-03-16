# CS 可视化模式参考

教学中需要可视化时，按此文档的格式决策、设计规范和生成规则执行。

---

## 1. 格式决策树

**先判断格式，再决定是否生成 HTML。默认选轻量格式。**

```
概念有 3+ 离散状态/步骤，且时序、交互、并发是理解的关键？
  └─ YES → 交互式 HTML（生成成本高，仅限真正需要动起来的概念）

结构性展示，无时间维度（如类型关系、内存布局）？
  └─ YES → ASCII 图（对话内直接嵌入）

标准流程图 / 序列图 / 类图？
  └─ YES → Mermaid 代码块（多数 Markdown 渲染器支持）

简单对比（2-3 项属性差异）？
  └─ YES → Markdown 表格
```

> 生成 HTML 是高成本操作。如果一张 ASCII 图能讲清楚，就别生成 HTML。

---

## 2. 设计系统

所有交互式 HTML 必须遵循统一视觉规范，确保跨演示风格一致。

### 色板

| 语义 | 色值 | 用途 |
|------|------|------|
| 背景 | `#0d1117` | body background |
| 面板 | `#161b22` | panel / card background |
| 边框 | `#30363d` | border, divider |
| 弱文字 | `#8b949e` | labels, descriptions |
| 主文字 | `#c9d1d9` | body text |
| 🟢 成功/运行 | `#4ade80` bg:`#1a2f1a` | running, success, done |
| 🔴 错误/阻塞 | `#ef4444` bg:`#2a1a1a` | blocked, error, busy |
| 🔵 主色/焦点 | `#58a6ff` bg:`#0f1a2e` | primary, counter, focus |
| 🟠 警告/锁定 | `#f97316` bg:`#1f1a14` | warning, locked |
| 🟡 添加/高亮 | `#fbbf24` bg:`#1f1a0a` | add, highlight |
| 🟣 特殊/等待 | `#a78bfa` bg:`#1a1a2e` | waiting, special |
| 🩵 信息/睡眠 | `#22d3ee` bg:`#0f1a2a` | info, sleep |

### 字体与排版

- 字体栈：`'SF Mono','Fira Code',monospace`
- 标题：18-20px bold
- 正文：11-12px
- 标签：9-10px
- 计数器大字：36px bold
- 代码：`color:#22d3ee`，inherit font

### 间距与圆角

- 面板 padding：14-20px
- 按钮 padding：6-8px 10-12px
- gap：6-14px
- border-radius：面板 8-10px，按钮 6px，小元素 4px

### 动画

- 状态过渡：`transition: all 0.3s-0.4s ease`
- 脉冲/呼吸：`@keyframes pulse { 50% { transform:scale(1.1) } }` 用于活跃状态
- 弹跳：`@keyframes jump { 50% { transform:scale(1.15) } }` 用于计数器变化
- 淡入：`@keyframes fi { from{opacity:0} to{opacity:1} }` 用于日志条目

### 实体状态 CSS 类

```css
.idle    { background:#161b22; border-color:#30363d; color:#6b7280; }
.running { background:#1a2f1a; border-color:#4ade80; color:#4ade80; }
.done    { background:#0f1a2e; border-color:#58a6ff; color:#58a6ff; }
.blocked { background:#2a1a1a; border-color:#ef4444; color:#ef4444; }
.waiting { background:#1a1a2e; border-color:#a78bfa; color:#a78bfa; }
.warning { background:#1f1a14; border-color:#f97316; color:#f97316; }
```

---

## 3. 可视化模式目录

| 模式 | 适用场景 | 布局骨架 | 关键元素 |
|------|----------|----------|----------|
| **State Machine** | 实体生命周期、协议 FSM、goroutine 状态 | 舞台区 + 居中资源框 + 四周实体圆/卡 | Entity Card × N + 资源框 + 状态切换动画 |
| **Comparison** | A vs B 对比、优劣权衡 | `grid: 1fr 1fr` 双面板，各含独立舞台+日志 | 双面板 + badge(good/bad) + 独立控制 |
| **Data Structure Ops** | 数组/树/链表/哈希操作 | 数组格子行 或 节点+连线 | 格子 div + 高亮活跃/比较项 + 指针标记 |
| **Algorithm Trace** | 排序、DP 表填充、搜索逐步 | 网格/表格 + 指针行 + 变量快照 | Grid cells + pointer arrows + step counter |
| **Timeline / Sequence** | 并发事件、消息传递、请求流 | 纵向生命线列 + 横向消息箭头 | Column divs + arrow labels + 激活条 |
| **Architecture** | 系统设计、微服务、网络拓扑 | 节点框 + 连接线 (CSS/SVG) | Node cards + connection lines + 数据流指示 |
| **Flow / Pipeline** | 编译管线、HTTP 请求生命周期、数据流 | 横向阶段框 + 箭头连接 | Stage boxes + arrow connectors + 高亮当前阶段 |
| **Tree / Graph** | 遍历可视化、最短路径、生成树 | 节点圆 + 边线 + 访问队列 | Node circles + edges + 颜色编码访问状态 |

**参考实现：**
- State Machine + Comparison → `docs/go-internals/mutex-vs-cond.html`
- State Machine + Counter → `docs/go-internals/waitgroup.html`

---

## 4. 组件原语

### 4.1 Entity Card（状态卡）

```html
<div class="g-card idle" id="w1">
  <div class="g-name">G2 · Worker 1</div>
  <div class="g-state" id="w1s">等待启动</div>
  <div class="g-prog"><div class="g-prog-inner" id="w1p"></div></div>
</div>
```
JS：`function setState(id, stateClass, label) { ... }`

### 4.2 Event Log（事件日志）

```html
<div class="log" id="log"></div>
```
JS：`function addLog(logId, cssClass, msg)` — 创建 `div.le`，含 `span.ts` 时间戳 + `span.{ok|warn|info|dim}` 消息。自动滚动到底部。

### 4.3 Control Bar（控制栏）

```html
<div class="controls">
  <button onclick="step1()" id="btn1">① 步骤一</button>
  <button onclick="step2()" id="btn2" disabled>② 步骤二</button>
  <button class="btn-rst" onclick="doReset()">↺ 重置</button>
</div>
<button class="btn-auto" onclick="doAuto()">▶ 自动演示</button>
```
渐进式启用：后续按钮 disabled，前一步完成后才 enable。
自动演示：`setInterval` 800-1200ms，toggle 开关，reset 时 `clearInterval`。

### 4.4 Counter Display（计数器）

```html
<div class="counter-box" id="cbox">
  <div class="counter-label">标签</div>
  <div class="counter-val" id="cval">0</div>
</div>
```
JS：`function setCounter(n, mode)` — mode='add' 黄色弹跳，'decr' 紫色弹跳，归零变绿。

### 4.5 Queue / Array Visualizer（队列/数组）

```html
<div class="qslots">
  <div class="qs" id="s0"></div>
  <div class="qs" id="s1"></div>
  ...
</div>
```
`.qs.on` 表示有数据，`.qs.active` 表示当前操作位。
JS：`function updateSlots(arr)` — 遍历更新。

### 4.6 Progress / CPU Bar（指标条）

```html
<div class="cpubar">
  <span>CPU</span>
  <div class="cpufill"><div class="cpuinner" id="bar"></div></div>
  <span id="barl">0%</span>
</div>
```
JS：`function setBar(innerId, labelId, pct, color)`

### 4.7 Info Card（说明卡）

```html
<div class="info-row"><!-- grid: 1fr 1fr [1fr] -->
  <div class="info-card">
    <h4 style="color:#fbbf24">标题</h4>
    <div class="nt">说明内容，可含 <code>代码</code></div>
  </div>
</div>
```

---

## 5. 领域→模式映射

| CS 领域 | 主模式 | 次模式 | 示例主题 |
|---------|--------|--------|----------|
| OS / 并发 | State Machine | Comparison, Timeline | goroutine 状态、进程调度、锁机制 |
| 计算机网络 | Timeline / Sequence | Architecture | TCP 握手、HTTP 请求流、路由 |
| 算法（排序/搜索/DP） | Algorithm Trace | Comparison | 快排分区、DP 表填充、二分查找 |
| 数据结构 | Data Structure Ops | Algorithm Trace | BST 增删、哈希冲突、堆操作 |
| 分布式系统 | Architecture | Timeline, Comparison | Raft 选举、2PC、一致性哈希 |
| 数据库 | Data Structure Ops | Flow | B+ 树插入、查询执行计划 |
| 编译器 / 语言 | Flow / Pipeline | Algorithm Trace | 词法→语法→AST→代码生成 |
| AI / ML | Flow / Pipeline | Data Structure Ops | Transformer 前向传播、注意力矩阵 |
| 系统设计 | Architecture | Comparison | 负载均衡、缓存分层、微服务 |
| 密码学 / 安全 | Flow / Pipeline | Timeline | 加密流程、TLS 握手、零知识证明 |

---

## 6. 生成规则清单

### 必须

- [ ] 自包含单 HTML 文件，零外部依赖
- [ ] `<!DOCTYPE html>` + `<meta charset="UTF-8">` + `lang` 属性匹配用户语言
- [ ] **ES5 JavaScript**：只用 `var`、`function`，不用 `=>`、`` ` ``、`let`、`const`、解构
- [ ] 所有 CSS 在 `<style>` 内，所有 JS 在 `<script>` 内
- [ ] 暗色主题，使用第 2 节色板
- [ ] 字体栈 `'SF Mono','Fira Code',monospace`
- [ ] 控制栏：编号步骤按钮 + 重置按钮 + 自动演示按钮
- [ ] 自动演示：`setInterval` 800-1200ms，可 toggle 开/关，重置时 `clearInterval`
- [ ] 事件日志面板：带时间戳、颜色分类
- [ ] 底部说明卡：2-3 张，解释核心概念
- [ ] CSS transition 动画（0.3-0.4s ease）
- [ ] 渐进式按钮启用（后步 disabled 直到前步完成）
- [ ] 重置函数清除所有状态、日志、定时器、恢复按钮初始状态
- [ ] 文本语言与用户对话一致

### 禁止

- [ ] ES6+ 语法（`=>`、模板字符串、`let`/`const`、解构、`class`）
- [ ] 外部 CDN、图片、字体链接
- [ ] `eval()`
- [ ] 直接拼接用户输入到 `innerHTML`（XSS 风险）

### 文件约定

- 文件名：`{topic-slug}.html`（小写、连字符分隔）
- 存放位置：`docs/{domain}/`（如 `docs/go-internals/`、`docs/algorithms/`、`docs/networking/`）
- **写完文件后必须立即启动本地服务并给出可点击链接**，流程如下：
  1. 在文件所在目录后台启动服务：`python3 -m http.server {port} --bind 127.0.0.1`（port 随机选 8000-8999 中未占用的一个）
  2. 在回复中输出 Markdown 链接：`[点击打开动画](http://127.0.0.1:{port}/{filename}.html)`
  3. 提示用户：若链接无法打开，可手动运行上方命令后刷新
