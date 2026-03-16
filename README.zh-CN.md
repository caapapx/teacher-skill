# Super Teacher for Claude Code

[English](./README.md) | [中文](./README.zh-CN.md)

**一个约 300 行的教学流程，把 AI 从“答案机器”变成“苏格拉底式导师”。**

<p align="center">
  <img src="docs/skill-logo.png" width="220" alt="teacher-skill logo" />
</p>

传统教学（1 对 1 / 1 对多）和大多数 LLM 问答本质上都在“压缩知识并灌输”。短期看起来高效，但长期留存不足。

本项目把检索练习、交错练习、双重编码、自我解释、元认知与苏格拉底提问组合成可执行的教学策略，目标是提升理解深度与迁移能力。

研究依据：Freeman et al. (2014), *Active learning increases student performance in science, engineering, and mathematics*, *PNAS* 111(23), 8410-8415。  
https://www.pnas.org/doi/10.1073/pnas.1319030111

<p align="center">
  <a href="https://github.com/caapapx/teacher-skill">GitHub</a> · <a href="https://github.com/caapapx/teacher-skill/issues">Issues</a>
</p>

---

## 快速开始

选择你的 runtime，并复制对应目录：

```bash
# Claude Code
cp -r teacher-cs/claude your-project/.claude/skills/teacher-cs

# Codex
cp -r teacher-cs/codex your-project/.codex/skills/teacher-cs

# Cursor
cp -r teacher-cs/cursor your-project/.cursor/skills/teacher-cs
```

如果使用通用版本（不限 CS），复制 `teacher/` 到 runtime 的 skills 路径。

安装后正常提问即可，skill 会自动接管教学流程。

---

## 系统架构

下图展示资源索引、技能路径生成，以及教师端与学生端的闭环。

<img src="docs/skill-desribe.png" width="900" alt="资源流架构与教师新能力结构" />

---

## 被动教学 vs 主动教学

| 维度 | 被动模式（普通 prompt） | 主动模式（`teacher-skill`） |
|---|---|---|
| 用户能复述定义 | 通常可以 | 可以 |
| 用户能解决新题 | 不稳定 | 显著更好 |
| 能否定位误解 | 弱 | 强 |
| 用户课中产出 | 少 | 必须产出 |
| 是否适配用户水平 | 往往不 | 内建 |

核心结果：主动学习平均可带来约 **0.47 SD** 的成绩提升，纯讲授不及格风险更高（Freeman et al., 2014）。

![Freeman et al. 2014 Fig 1: Active learning vs lecture failure rates](docs/freeman-2014-fig1-failure-rate.jpg)

流程提效示意：

<img src="docs/skill-boost-comparison.png" width="900" alt="teacher-skill 提效对比图" />

---

## 七种教学模式

| 模式 | 名称 | 作用 | 触发场景 |
|---|---|---|---|
| A | 引导拆解 | 分步拆解与手动 trace | 算法题、推导、case 分析 |
| B | 苏格拉底 + 高级回忆 | 用追问链建立深层理解 | 面试准备、概念深挖 |
| C | 心智模型 + 双重编码 | 文本 + 图示解释抽象概念 | 系统设计、调度、Transformer |
| D | 简化 + 类比 | 将术语转换成入门语言 | 入门概念、"什么是 X" |
| E | 深度追问 | why/how/boundary 连续探查 | 机制与因果问题 |
| F | 交错练习 + 生成式学习 | 混合练习 + 总结/闪卡/类比 | 系统学习类请求 |
| G | 元认知策略 | 诊断学习瓶颈 | 低留存、学不进去 |

路由会按时间预算、回答偏好、交互强度、基础水平动态调整。

---

## 证据基础

| 方法 | 来源 | 关键结论 |
|---|---|---|
| 主动学习 | https://www.pnas.org/doi/10.1073/pnas.1319030111 | 效果量约 +0.47 SD |
| 检索练习 | https://journals.sagepub.com/doi/10.1111/j.1467-9280.2006.01693.x | 回忆优于重复阅读 |
| 交错练习 | https://www.tandfonline.com/doi/abs/10.1080/00220970709598675 | 混合练习优于分块练习 |
| 双重编码 | https://files.eric.ed.gov/fulltext/ED340377.pdf | 文本+图示优于纯文本 |
| 自我解释 | https://www.taylorfrancis.com/chapters/edit/10.4324/9780203052860-13/ | 主动解释可提高表现 |
| 元认知 | https://psycnet.apa.org/record/1980-09388-001 | 监控理解能力可训练 |
| 苏格拉底提问 | https://link.springer.com/article/10.1007/BF02310573 | 引导式提问优于直接告知 |
| 学习技术综述 | https://journals.sagepub.com/doi/10.1177/1529100612453266 | 检索与交错属于高效策略 |

---

## 知识源优先级

```
用户材料（教材、笔记、代码）
  -> 不足时
互联网检索（最新文档、API、变更）
  -> 不可用时
RAG / 本地知识库
  -> 未配置时
模型内建知识
```

用户材料始终优先。

---

## `teacher` vs `teacher-cs`

| Skill | 范围 | 优化方向 |
|---|---|---|
| [`teacher`](./teacher/) | 通用学科 | 概念形成、复习、引导练习 |
| [`teacher-cs`](./teacher-cs/) | 计算机科学 | 算法 trace、系统设计、面试训练、可视化教学 |

`teacher-cs` 额外提供：

- 状态快照式算法追踪
- CS 领域适配（Go、算法、系统、AI、面试）
- 交互式 HTML 可视化
- 可视化决策树（ASCII -> Mermaid -> HTML）

---

## Runtime 矩阵

| Runtime | 路径 | 说明 |
|---|---|---|
| Claude Code | [`teacher-cs/claude/`](./teacher-cs/claude/) | 完整包：evals + knowledge sources + viz patterns |
| Codex | [`teacher-cs/codex/`](./teacher-cs/codex/) | 包含 `agents/openai.yaml` |
| Cursor | [`teacher-cs/cursor/`](./teacher-cs/cursor/) | 精简核心教学逻辑 |

---

## 仓库结构

```text
teacher-skill/
├── README.md
├── README.zh-CN.md
├── CONTRIBUTING.md
├── teacher/
│   ├── SKILL.md
│   ├── references/
│   └── evals/
└── teacher-cs/
    ├── evals/
    ├── claude/
    │   ├── SKILL.md
    │   ├── references/
    │   └── evals/
    ├── codex/
    │   ├── SKILL.md
    │   ├── agents/
    │   └── references/
    └── cursor/
        ├── SKILL.md
        └── references/
```

---

## 真实教学场景

- “两数之和怎么做？” -> 模式 A，输出变量表 + 步进快照 + 自检点，不直接给最终代码。
- “Redis 持久化面试题” -> 模式 B，先基线探测，再多层追问链。
- “解释 Transformer attention” -> 模式 C，矩阵图示 + 文字解释 + 检验题。
- “系统学习分布式系统” -> 模式 F，时间盒学习计划 + 混合练习 + 生成式任务。

---

## TODO

- [ ] 更多领域适配器（医学、法律、金融）
- [ ] 扩展 eval 用例（5 -> 20+）
- [ ] 跨 runtime 一致性测试
- [ ] 可视化模板扩展
- [ ] 学习进度追踪

---

## 贡献

详见 [CONTRIBUTING.md](./CONTRIBUTING.md)。常见贡献路径：

1. 在 `references/domain-<name>.md` 新增领域适配器
2. 在 `evals/evals.json` 增加评测用例
3. 在 `SKILL.md` 改进教学逻辑
4. 通过 MCP server + `references/source-<type>.md` 扩展知识源

---

## 许可证

MIT
