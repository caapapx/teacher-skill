# Contributing to skills

Thank you for your interest in contributing! This guide covers how to add new skills, domain adapters, eval cases, and knowledge source extensions.

---

## License

This project uses the **MIT License** — the most permissive option, with the lowest barrier for community contributions and rapid adoption.

---

## Contribution Types

### 1. Add a New Domain Adapter

Domain adapters let the `teacher` skill optimize its teaching strategy for specific domains.

1. Create `teacher/references/domain-<name>.md` (e.g., `domain-medicine.md`, `domain-law.md`)
2. Follow the structure in `teacher/references/domains.md`:
   - Domain name and brief description
   - 3–5 high-frequency learning scenarios and recommended teaching modes
   - Domain-specific teaching notes
   - Recommended authoritative knowledge sources
3. Open a PR

### 2. Add Eval Cases

Add test cases to `evals/evals.json` in either skill:

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

### 3. Improve a Skill

Edit `SKILL.md` directly. The `description` frontmatter controls when the skill auto-triggers — keep it precise. Be conservative: each addition should earn its place.

### 4. Add a Knowledge Source Extension

1. Write a new MCP Server and document the integration
2. Add `references/source-<type>.md` describing the adapter protocol
3. The skill's knowledge source management section will reference it automatically

---

## Cross-Model Compatibility Notes

The skills are designed to work across multiple runtimes and AI environments. Key design decisions:

| Concern | Solution |
|---------|---------|
| Different search tool names per platform | SKILL.md describes "use whatever search tool is available in the current environment" — no hardcoded tool names |
| Different file reading methods | Uses generic description "read the file" — let the model choose available tools |
| MCP availability varies | `knowledge-sources.md` defines a graceful degradation strategy |
| Multimodal support varies | ASCII diagrams as the baseline visual representation; image generation as an enhancement |
| Runtime-specific metadata | Keep runtime-only files inside runtime folders such as `teacher-cs/codex/agents/` |
| Runtime packaging differences | `teacher-cs/claude`, `teacher-cs/codex`, and `teacher-cs/cursor` stay self-contained for copy-install |

---

## Keeping Private Knowledge Private

The teacher skills themselves contain no private knowledge — all teaching content comes from:
1. Materials the user provides at runtime (never enters skill files)
2. Internet search results (fetched live, not persisted)
3. The model's built-in knowledge (public information)

If you build a domain adapter that references proprietary sources:
- Exclude it from version control via `.gitignore`
- Store private adapters under `references/private/` and note in your README that this directory should not be committed
- Consider keeping proprietary knowledge in a RAG/MCP layer rather than in skill files

---

## Before Opening a PR

- No private or sensitive information in any committed file
- New domain adapters follow the structure in `references/domains.md`
- Eval cases include explicit, verifiable assertions
- README and contribution docs should stay in English
- Runtime skill content may remain in the source language used by that runtime if that is the canonical upstream version
