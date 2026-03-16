# teacher-skill

**300 行教学逻辑，把 AI 从答案机器变成苏格拉底。**

传统的教学（1对1,1对多）或者说基于LLM提问的知识问答，做的事情是一样的：压缩知识、倾倒给用户。用户感觉高效，但一周后什么都不记得。

时代变了，“被动学习”已死，这个skill帮你提升200%+🚀以上的学习效率。它用 7 种循证教学策略（retrieval practice、interleaving、dual coding……）强迫用户的大脑做功。不是因为听起来高级，而是因为[论文来源](https://www.pnas.org/doi/10.1073/pnas.1319030111)。

一套教学哲学，三个 runtime，复制即用。

<p align="center">
  <a href="https://github.com/caapapx/teacher-skill">GitHub</a> · <a href="https://github.com/caapapx/teacher-skill/issues">Issues</a>
</p>

---

## Quick Start

选你的 runtime，一条命令装好：

```bash
# Claude Code
cp -r teacher-cs/claude your-project/.claude/skills/teacher-cs

# Codex
cp -r teacher-cs/codex your-project/.codex/skills/teacher-cs

# Cursor
cp -r teacher-cs/cursor your-project/.cursor/skills/teacher-cs
```

通用版（不限 CS）：把 `teacher/` 复制到对应 runtime 的 skills 路径。

装完之后，像平时一样提问。skill 会自动接管教学。

---

## 被动教学 vs 主动教学

| 维度 | 被动模式（传统 prompt） | 主动模式（teacher-skill） |
|------|----------------------|------------------------|
| 用户能复述定义？ | 通常可以 | 可以 |
| 用户能解新题？ | 不可靠 | 显著更好 |
| 系统能定位误解？ | 弱 | 强 |
| 用户在课中产出内容？ | 极少 | 强制 |
| 课程适配用户水平？ | 通常不 | 内建 |

核心数据：主动学习相比纯讲授，考试成绩提升约 **0.47 个标准差**，纯讲授课程不及格率显著更高（[Freeman et al., PNAS 2014](https://www.pnas.org/doi/10.1073/pnas.1319030111)）。

---

## 7 种教学模式

skill 不是一个技巧，是一个教学模式栈，根据用户意图自动切换。

| 模式 | 名称 | 做什么 | 触发场景 |
|------|------|--------|----------|
| A | 引导拆解 | 逐步分解问题，手动 trace | 算法题、数学推导、case 分析 |
| B | 苏格拉底 + 高级回忆 | 提问链驱动深层理解 | 面试准备、概念深挖 |
| C | 心智模型 + 双重编码 | 文字 + 图表解释复杂抽象 | 系统设计、GMP 调度、Transformer |
| D | 简化 + 类比 | 把术语墙变成 12 岁能懂的话 | 入门概念、"什么是 goroutine？" |
| E | 深度追问 | 连续 why/how/boundary 探测 | "为什么是这样？"类问题 |
| F | 交错练习 + 生成式学习 | 混合练习 + 总结/闪卡/类比 | "系统学习 X" |
| G | 元认知策略 | 诊断学习效率，推荐更好方法 | "我学不进去"、低留存 |

路由决策基于四个维度：时间预算、回答偏好、交互强度、基线水平。支持 session 内动态切换。

---

## 教学方法的学术依据

每个模式都映射到已发表的学习科学研究，不是 productivity hack。

| 方法 | 来源 | 关键发现 |
|------|------|----------|
| 主动学习 | [Freeman et al., 2014](https://www.pnas.org/doi/10.1073/pnas.1319030111) | 效果量 +0.47 SD，纯讲授不及格率更高 |
| 检索练习 | [Roediger & Karpicke, 2006](https://journals.sagepub.com/doi/10.1111/j.1467-9280.2006.01693.x) | 测试效应：回忆 > 重复阅读 |
| 交错练习 | [Rohrer & Taylor, 2007](https://www.tandfonline.com/doi/abs/10.1080/00220970709598675) | 混合练习优于分块练习 |
| 双重编码 | [Clark & Paivio, 1991](https://files.eric.ed.gov/fulltext/ED340377.pdf) | 文字 + 图像 > 纯文字 |
| 自我解释 | [Chi et al., 1989](https://www.taylorfrancis.com/chapters/edit/10.4324/9780203052860-13/) | 主动解释的学生表现显著更好 |
| 元认知 | [Flavell, 1979](https://psycnet.apa.org/record/1980-09388-001) | 监控自身理解的能力可以训练 |
| 苏格拉底提问 | [Graesser & Person, 1994](https://link.springer.com/article/10.1007/BF02310573) | 引导性提问优于直接告知 |
| 高效学习技术综述 | [Dunlosky et al., 2013](https://journals.sagepub.com/doi/10.1177/1529100612453266) | 检索和交错是最高效用技术 |

---

## 知识源优先级

教学内容的来源决定教学质量。skill 按以下顺序选择知识源：

```
用户上传的材料（教材、笔记、代码）
  ↓ 不够时
互联网搜索（最新文档、API、breaking changes）
  ↓ 不可用时
RAG / 本地知识库（企业私有数据）
  ↓ 未配置时
模型内建知识（经典理论、基础知识）
```

用户的材料永远是第一优先。skill 的角色是帮用户掌握*他们的*材料，而不是替换。

---

## teacher vs teacher-cs

| skill | 适用范围 | 优化方向 |
|-------|----------|----------|
| [`teacher`](./teacher/) | 任意学科 | 概念形成、复习、引导练习 |
| [`teacher-cs`](./teacher-cs/) | 计算机科学 | 算法 trace、系统设计、runtime 内部、面试、可视化教学 |

`teacher-cs` 额外包含：

- **算法可视化** — 状态快照式 hand-trace，不只是伪代码
- **CS domain adapter** — Go / 算法 / 系统设计 / AI / 面试的专属教学策略
- **交互式 HTML 可视化** — 并发系统、状态机、数据结构操作
- **可视化设计系统** — ASCII → Mermaid → HTML 的复杂度决策树，GitHub 暗色主题配色

---

## Runtime 矩阵

| Runtime | 路径 | 特点 |
|---------|------|------|
| Claude Code | [`teacher-cs/claude/`](./teacher-cs/claude/) | 完整版：evals + knowledge-sources + viz-patterns |
| Codex | [`teacher-cs/codex/`](./teacher-cs/codex/) | 含 `agents/openai.yaml`，Codex 适配 prompt |
| Cursor | [`teacher-cs/cursor/`](./teacher-cs/cursor/) | 精简版，核心教学逻辑 |

一套教学哲学，三个自包含包，复制即安装，零翻译成本。

---

## 仓库结构

```
teacher-skill/
├── README.md
├── CONTRIBUTING.md
├── teacher/                  # 通用学习教练
│   ├── SKILL.md              # 教学逻辑（309 行）
│   ├── references/           # 领域适配协议
│   └── evals/                # 5 个基准测试用例
└── teacher-cs/               # CS 专业版
    ├── evals/                # 共享评估用例
    ├── claude/               # Claude Code 完整包
    │   ├── SKILL.md
    │   ├── references/       # domains + knowledge-sources + viz-patterns
    │   └── evals/
    ├── codex/                # Codex 适配包
    │   ├── SKILL.md
    │   ├── agents/
    │   └── references/
    └── cursor/               # Cursor 适配包
        ├── SKILL.md
        └── references/
```

---

## 真实教学场景

**"两数之和怎么做？"** → 路由到模式 A（引导拆解）
- 输出：变量表 + 状态快照式 hand-trace + 关键洞察标记 + 自检清单
- 不直接给代码

**"Redis 持久化面试题"** → 路由到模式 B（苏格拉底）
- 输出：基线探测 → 3-5 层追问链 → 用户自行发现知识盲区

**"解释 Transformer 的 attention"** → 路由到模式 C（心智模型 + 双重编码）
- 输出：Q/K/V 矩阵 ASCII 图 + 文字解释 + 1-2 道检验题
- 用户必须作答才能继续

**"系统学习分布式系统"** → 路由到模式 F（交错练习）
- 输出：30 分钟学习计划 + 混合概念题 + 生成式任务（总结/类比/闪卡）

---

## TODO

- [ ] 更多学科的 domain adapter（医学、法律、金融）
- [ ] eval 用例从 5 个扩展到 20+
- [ ] 跨 runtime 一致性自动化测试
- [ ] 可视化模板库扩展
- [ ] 学习进度追踪机制

---

## 贡献

详见 [CONTRIBUTING.md](./CONTRIBUTING.md)。四种贡献路径：

1. **新 domain adapter** — 创建 `references/domain-<name>.md`
2. **eval 用例** — 添加到 `evals/evals.json`
3. **改进 skill** — 直接编辑 `SKILL.md`
4. **知识源扩展** — 写 MCP Server + `references/source-<type>.md`

---

## 致谢

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://openai.com/codex/)
- [Cursor](https://www.cursor.com/)

---

## License

MIT
