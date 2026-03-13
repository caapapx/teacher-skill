# 开源发布指南

## License 建议

推荐使用 **MIT License** 或 **Apache 2.0**：
- MIT：最宽松，社区贡献门槛最低，适合快速传播
- Apache 2.0：包含专利授权条款，对企业用户更友好

## 防止 Prompt 泄露私有知识

teacher skill 本身不包含任何私有知识，所有教学内容来自：
1. 用户运行时提供的材料（不进入 skill 文件）
2. 互联网搜索结果（实时获取，不持久化）
3. 模型内置知识（公开信息）

但如果用户基于 teacher skill 创建了包含私有知识的领域适配文件，需要注意：
- 在 `.gitignore` 中排除包含私有知识的 reference 文件
- 使用 `references/private/` 子目录存放私有适配文件，并在 README 中说明此目录不应提交
- 考虑将私有知识放在 RAG/MCP 层而非 skill 文件中

## 跨模型工具调用差异

不同模型平台的工具调用方式不同，teacher skill 通过以下方式处理：

| 差异点 | 处理方式 |
|--------|----------|
| 搜索工具名称不同 | SKILL.md 中不硬编码工具名，而是描述"使用当前环境中可用的搜索工具" |
| 文件读取方式不同 | 使用通用描述"读取文件"，让模型自行选择可用工具 |
| MCP 可用性不同 | knowledge-sources.md 中定义了降级策略 |
| 多模态支持不同 | 视觉内容（ASCII 图）作为兜底，图片生成作为增强 |

## 目录结构

```
teacher/                              # 通用版（领域无关）
├── SKILL.md                          # 核心指令
├── PUBLISHING.md                     # 本文件
├── references/
│   ├── domains.md                    # 通用领域适配协议
│   └── knowledge-sources.md          # 知识源适配协议
└── evals/
    └── evals.json                    # 评估基准

teacher-cs/                           # CS 专精版
├── SKILL.md                          # CS 领域核心指令
├── references/
│   ├── domains.md                    # CS 领域适配（算法、系统设计、AI 等）
│   └── knowledge-sources.md          # 知识源适配协议（共享）
└── evals/
    └── evals.json                    # CS 领域评估基准
```

## 安装方式

### 方式 1：手动安装
```bash
# Claude Code
cp -r teacher/ .claude/skills/teacher/

# Cursor
cp -r teacher/ .cursor/skills/teacher/

# Codex
cp -r teacher/ .codex/skills/teacher/
```

### 方式 2：CS 专精版
```bash
# 同时安装通用版和 CS 版
cp -r teacher/ .claude/skills/teacher/
cp -r teacher-cs/ .claude/skills/teacher-cs/
```

## 社区贡献指南

欢迎贡献：
1. **新领域适配文件** — 在 `references/` 下添加 `domain-<领域>.md`
2. **新评估用例** — 在 `evals/evals.json` 中添加新的 test case
3. **知识源适配器** — 在 `references/` 下添加 `source-<类型>.md`
4. **Bug 修复和教学流程优化** — 直接修改 SKILL.md

贡献前请确保：
- 不包含任何私有/敏感信息
- 新领域适配文件遵循 `domains.md` 中的通用领域适配协议格式
- 评估用例包含明确的 assertions
