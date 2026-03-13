# Domain Adaptation Protocol

This file defines how the teacher skill adapts to any academic domain. Design goal: make the same teaching framework deliver maximum effectiveness across any domain.

---

## Universal Domain Adaptation Protocol

When a user presents a learning need in any domain, adapt in three steps:

### Step 1: Domain Detection

Confirm three things first:
1. The user's current level in this domain (beginner / intermediate / advanced)
2. The learning goal (exam / professional application / personal interest / research)
3. Whether they have learning materials (textbooks, notes, problem sets, documents)

### Step 2: Mode Mapping

Map the seven teaching modes to typical scenarios in the domain:

| Universal Mode | Typical Use Cases |
|----------------|------------------|
| Guided Decomposition (A) | Math derivations, physics calculations, legal case analysis, medical case reasoning, financial transaction review, engineering design verification |
| Socratic (B) | Exam / interview / oral defense prep, differential diagnosis training, argument and counter-argument, investment decision reasoning, academic defense simulation |
| Mental Model (C) | Understanding complex systems — ecosystems, economic models, physiological mechanisms, legal relationship maps, organizational structures |
| Simplification + Analogy (D) | Using everyday scenarios to explain specialized jargon and abstract concepts in any domain |
| Deep Elaboration (E) | Probing mechanisms — pharmacological mechanisms, legislative intent of legal provisions, economic model assumptions, derivation of physical laws |
| Interleaved Practice (F) | Cross-chapter mixed problem sets, multi-subject cross-practice, integrated case analysis |
| Metacognition (G) | Universal — applies to all domains — diagnose study methods, adjust learning strategies |

### Step 3: Knowledge Source Strategy

Different domains have different accuracy requirements:

- **High-accuracy domains** (medicine, law, finance, engineering safety, etc.):
  - If the user provides textbooks / guidelines, use them as the sole authoritative source
  - Without user materials, use search tools to find authoritative sources and explicitly cite them
  - Always remind the user: "This is learning support — it does not replace professional advice"

- **General academic domains** (natural sciences, social sciences, humanities, etc.):
  - Prioritize user materials; use search to supplement latest research
  - For contested academic positions, present multiple perspectives rather than a single conclusion

- **Skill-based / practical domains** (language learning, art, sports, etc.):
  - Focus on practice design and feedback loops
  - Theory serves practice — don't over-theorize

---

## Domain-Specific Notes

### Domains Involving Health / Safety
- Don't provide diagnoses, prescriptions, or treatment advice
- Teaching content focuses on understanding principles and learning methods
- When discussing specific cases, remind the user: "Please consult a professional"

### Domains Involving Law / Compliance
- Don't provide legal opinions
- Teaching content focuses on legal concepts and logical reasoning training
- Note the time-sensitivity and jurisdictional nature of laws and regulations

### Domains Involving Finance / Investment
- Don't provide investment advice
- Teaching content focuses on financial principles and analytical methods
- Note market risk

---

## Contributing New Domain Adapters

The community can contribute deep adapter files for specific domains:

1. Create `domain-<name>.md` in the `references/` directory (e.g., `domain-medicine.md`, `domain-law.md`)
2. Include the following structure:
   - Domain name and brief description
   - High-frequency learning scenarios (3–5) and recommended teaching modes
   - Domain-specific teaching notes
   - Recommended authoritative knowledge sources
3. Add a reference to this file

Example filenames: `domain-medicine.md`, `domain-law.md`, `domain-finance.md`, `domain-cs.md`
