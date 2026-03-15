# Teacher skill — A powerful skill that helps you learn everything while using Claude Code.

A curated registry of Claude Code skills for learning, teaching, and technical mastery. Drop any skill into your project's `.claude/skills/` directory and it activates automatically.

<p align="center">
  <a href="https://github.com/caapapx/teacher-skill">GitHub</a> · <a href="https://github.com/caapapx/teacher-skill/issues">Bug & Feature</a>
</p>

---

## What is a Claude Code Skill?

A skill is a Markdown instruction file that Claude Code loads from `.claude/skills/<name>/SKILL.md`. When the `description` frontmatter matches user intent, Claude automatically applies the skill's behavior — no slash commands required.

```
your-project/
└── .claude/
    └── skills/
        └── teacher/
            └── SKILL.md   ← Claude reads this automatically
```

---

## Skills

| Skill | Trigger | What it does |
|---|---|---|
| [teacher](./teacher/) | `/teacher`, or "I want to learn / understand / review X" | Universal learning coach for any domain — science, law, finance, language, engineering |
| [teacher-cs](./teacher-cs/) | `/teacher-cs`, or "help me study algorithms / system design / interview prep" | CS-specialized variant — algorithms, programming languages, system design, AI/LLM, technical interviews |

---

## Quick Start

### Install a single skill

```bash
# Clone the registry
git clone https://github.com/caapapx/skills.git

# Copy a skill into your project
cp -r skills/teacher your-project/.claude/skills/teacher
```

### Install all skills

```bash
cp -r skills/* your-project/.claude/skills/
```

### Trigger in Claude Code

```
/teacher explain gradient descent, I have 20 minutes

/teacher-cs help me understand two-sum, guided walkthrough

/teacher I want to study contract law for the bar exam
```

---

## Skill Architecture

```
skill/
├── SKILL.md                  # Core instruction (frontmatter + prompt)
├── references/
│   ├── domains.md            # Domain-specific routing hints
│   └── knowledge-sources.md  # Knowledge source priority (user docs > search > model)
└── evals/
    └── evals.json            # Evaluation cases and assertions
```

### How teacher routing works

```
User input
  │
  ├─ Step-by-step problem / derivation?     → Mode A: Guided Decomposition
  ├─ "What is X" / conceptual question?     → Mode C/D: Mental Model + Analogy
  ├─ Exam / interview / defense prep?       → Mode B: Socratic + Recall
  ├─ "Why does X work" / mechanism?         → Mode E: Deep Elaboration
  ├─ Systematic study of a topic?           → Mode F: Interleaved Practice
  └─ Feeling stuck / low-efficiency?        → Mode G: Metacognition
```

### Knowledge source priority

```
User-provided files → Local project docs → Internet search → RAG/MCP → Model knowledge
      (highest)                                                            (fallback)
```

---

## teacher vs teacher-cs

| Feature | teacher | teacher-cs |
|---|---|---|
| Domain | Any subject | Computer Science only |
| Algorithm visualization | Generic | Code trace + variable table |
| Interview prep | General exam/oral defense | Technical interview (LeetCode, system design, OS) |
| Code execution | Optional | Central to teaching flow |
| Domain references | `domains.md` (universal protocol) | `domains.md` (CS-specific: algorithms, distributed systems, AI) |
| Eval cases | Math, biology, economics, law, linguistics | Algorithms, Redis, Transformer, Go runtime, LLM internals |

---

## Repository Structure

```
skills/
├── README.md
├── CONTRIBUTING.md                # Contribution guide & cross-model compatibility notes
├── teacher/                       # Universal learning coach
│   ├── SKILL.md                   # Core instructions (7 teaching modes)
│   ├── references/
│   │   ├── domains.md             # Universal domain routing protocol
│   │   └── knowledge-sources.md   # Knowledge source adapter protocol
│   └── evals/
│       └── evals.json             # Eval cases across 6+ domains
└── teacher-cs/                    # CS-specialized learning coach
    ├── SKILL.md                   # CS-tuned instructions
    ├── references/
    │   ├── domains.md             # CS domain routing (algorithms, system design, AI)
    │   └── knowledge-sources.md   # Knowledge source adapter protocol
    └── evals/
        └── evals.json             # CS eval cases (algorithms, interviews, LLM)
```

---

## Contributing

### Add a new domain adapter

1. Create `teacher/references/domain-<name>.md` (e.g. `domain-medicine.md`, `domain-law.md`)
2. Follow the structure in `references/domains.md` — include: domain name, top 3–5 learning scenarios, recommended teaching modes, and authority source notes
3. Open a PR

### Add eval cases

Add entries to `evals/evals.json` with the format:

```json
{
  "id": 99,
  "name": "short-descriptive-name",
  "prompt": "what the user would type",
  "expected_output": "which mode should activate and why",
  "assertions": ["list of verifiable behaviors"],
  "domain": "domain-name",
  "tests_mode": "A"
}
```

### Improve the skill

Edit `SKILL.md` directly. The description frontmatter controls when the skill auto-triggers — keep it precise. See `CONTRIBUTING.md` for cross-model compatibility notes.

---

## Acknowledgements

| Project | Role |
|---|---|
| [Claude Code](https://github.com/anthropics/claude-code) | Skill runtime |
| [letsgo](https://github.com/caapap/letsgo) | Origin project where teacher skill was developed |
| [gameye.org](https://github.com/caapapx/gameye.org) | Parent organization |

---

## License

MIT
