# teacher-skill

most learning prompts still do the same dumb thing: compress answers and dump them on the user.

this repo is for the opposite style.

it packages teacher skills that force recall, comparison, self-explanation, visual reasoning, and metacognitive checks. not because that sounds nice. because the evidence is better.

<p align="center">
  <a href="https://github.com/caapapx/teacher-skill">GitHub</a> · <a href="https://github.com/caapapx/teacher-skill/issues">Issues</a>
</p>

---

## what is here

- `teacher/`: the general learning coach
- `teacher-cs/`: the computer-science version
- `teacher-cs/claude/`: Claude-ready runtime package
- `teacher-cs/codex/`: Codex-ready runtime package
- `teacher-cs/cursor/`: Cursor-ready runtime package
- `teacher-cs/evals/`: shared evaluation cases

the point is simple: one teaching philosophy, multiple runtimes, minimal translation cost.

---

## quick install

### claude code

```bash
cp -r teacher-skill/teacher-cs/claude your-project/.claude/skills/teacher-cs
```

### codex

```bash
cp -r teacher-skill/teacher-cs/codex your-project/.codex/skills/teacher-cs
```

### cursor

```bash
cp -r teacher-skill/teacher-cs/cursor your-project/.cursor/skills/teacher-cs
```

for the domain-agnostic coach, copy `teacher/` into the runtime path you use.

---

## runtime matrix

| runtime | path in this repo | notes |
|---|---|---|
| Claude Code | [`teacher-cs/claude/`](./teacher-cs/claude/) | includes `knowledge-sources` and `evals` from the Claude skill |
| Codex | [`teacher-cs/codex/`](./teacher-cs/codex/) | includes `agents/openai.yaml` and the Codex-tuned prompt |
| Cursor | [`teacher-cs/cursor/`](./teacher-cs/cursor/) | mirrors the Cursor skill layout |

full version notes live in [`teacher-cs/README.md`](./teacher-cs/README.md).

---

## teacher vs teacher-cs

| skill | target | what it optimizes for |
|---|---|---|
| [`teacher`](./teacher/) | any domain | concept formation, review, guided practice |
| [`teacher-cs`](./teacher-cs/) | computer science | algorithms, system design, runtime internals, interview prep, code-adjacent visual teaching |

---

## why this approach

traditional teaching is optimized for delivery.

these skills are optimized for retention and transfer.

that changes the design.

| mode | good at | weak at | evidence signal |
|---|---|---|---|
| traditional lecture / answer dump | fast compression | low retrieval, weak transfer, confidence inflation | active learning improves exam performance by about `0.47 SD`, and lecture-heavy classes show higher failure rates in the Freeman et al. meta-analysis ([PNAS](https://www.pnas.org/doi/10.1073/pnas.1319030111)) |
| worked examples only | lowering entry cost for novices | users can follow without being able to reproduce | strong for onboarding, but needs self-explanation or retrieval to stick ([Sweller and Cooper](https://link.springer.com/article/10.1007/BF00375122), [Chi et al.](https://www.taylorfrancis.com/chapters/edit/10.4324/9780203052860-13/self-explanations-how-students-study-use-examples-learning-solve-problems-michelene-chi-miriam-bassok-matthew-lewis-peter-reimann-robin-glaser)) |
| teacher-cs style guided teaching | slower per turn, much better diagnosis | needs user effort | combines retrieval, questioning, visualization, and reflection; this is closer to what the high-utility learning-technique literature recommends ([Dunlosky et al.](https://journals.sagepub.com/doi/10.1177/1529100612453266)) |

short version: passive teaching feels efficient. active teaching usually is efficient.

---

## benchmark: passive vs active

| benchmark question | passive pattern | teacher pattern |
|---|---|---|
| can the learner repeat the definition? | usually yes | yes |
| can the learner solve a fresh problem? | unreliable | much better |
| can the system locate misunderstanding early? | weak | strong |
| does the learner generate output during the lesson? | rare | mandatory |
| does the lesson adapt to user level and mistakes? | usually no | built in |

this repo leans hard into the right column.

---

## teaching models inside the skill

the skill is not one trick. it is a stack of teaching models, switched by user intent.

| model | what it does | official source | paper support |
|---|---|---|---|
| socratic questioning | uses guided questions to surface gaps instead of dumping the answer | [Encyclopaedia Britannica: Socratic method](https://www.britannica.com/topic/Socratic-method) | [Graesser & Person, 1994](https://link.springer.com/article/10.1007/BF02310573) |
| retrieval practice | makes the user recall before seeing the final explanation | [The Learning Scientists: Retrieval Practice](https://www.learningscientists.org/retrieval-practice) | [Roediger & Karpicke, 2006](https://journals.sagepub.com/doi/10.1111/j.1467-9280.2006.01693.x), [Karpicke & Blunt, 2011](https://www.science.org/doi/10.1126/science.1199327) |
| interleaving | mixes adjacent topics so the learner must discriminate, not just pattern-match | [The Learning Scientists: Interleaving](https://www.learningscientists.org/interleaving) | [Rohrer & Taylor, 2007](https://www.tandfonline.com/doi/abs/10.1080/00220970709598675), [Dunlosky et al., 2013](https://journals.sagepub.com/doi/10.1177/1529100612453266) |
| dual coding | explains with words plus diagrams, traces, or timelines | [The Learning Scientists: Dual Coding](https://www.learningscientists.org/dual-coding) | [Clark & Paivio, 1991](https://files.eric.ed.gov/fulltext/ED340377.pdf) |
| self-explanation / generative learning | forces the learner to restate, build analogies, or explain a worked example | [The Learning Scientists: Elaboration](https://www.learningscientists.org/elaboration) | [Chi et al., 1989](https://www.taylorfrancis.com/chapters/edit/10.4324/9780203052860-13/self-explanations-how-students-study-use-examples-learning-solve-problems-michelene-chi-miriam-bassok-matthew-lewis-peter-reimann-robin-glaser) |
| metacognitive coaching | asks the learner to inspect what feels solid, shaky, or missing | [Harvard Bok Center: Metacognition](https://bokcenter.harvard.edu/metacognition) | [Flavell, 1979](https://psycnet.apa.org/record/1980-09388-001) |

these are not random productivity hacks. they are the operating system of the skill.

---

## repository shape

```text
teacher-skill/
├── README.md
├── CONTRIBUTING.md
├── teacher/
│   ├── SKILL.md
│   ├── references/
│   └── evals/
└── teacher-cs/
    ├── README.md
    ├── evals/
    │   └── evals.json
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

## acknowledgements

- [Claude Code](https://github.com/anthropics/claude-code)
- [letsgo](https://github.com/caapap/letsgo)
- [OpenAI Codex](https://openai.com/codex/)
- [Cursor](https://www.cursor.com/)

---

## license

MIT
