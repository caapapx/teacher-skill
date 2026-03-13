# Domain Adaptation Reference

Different technical domains have different knowledge structures and optimal teaching entry points.

---

## Go Language

**High-frequency learning scenarios:**
- Concurrency model (goroutine, channel, select) → Mental Model + Dual Coding (draw the GMP scheduler diagram)
- Memory management (GC, escape analysis) → Deep Elaboration (probe "why tri-color marking?")
- Interfaces and type system → Simplification + Analogy (use real-world "contract" analogy)
- Standard library source reading → Deep Elaboration + Algorithm Visualization (step-by-step hand-trace)
- Performance optimization → Socratic (guide the user to locate the bottleneck, don't just give the answer)

**Go interview follow-up chain:**
goroutine leak → how to detect → how to use pprof → a real debugging case

## Algorithms and Data Structures

**Best mode: Algorithm Visualization (Mode A) as primary**

Key principles:
- Always hand-trace a small example first to build intuition
- Start from brute force, then optimize (let the user understand the motivation for optimization)
- Comparing similar problems is core (two-sum vs. three-sum — what's different?)
- Time / space complexity isn't just memorized — you should be able to derive it

**By problem type:**
| Problem Type | Emphasis |
|-------------|---------|
| Dynamic Programming | The "why" behind state transitions matters more than the formula |
| Trees / Graphs | Dual coding — drawing is a prerequisite for understanding |
| Two Pointers / Sliding Window | Hand-trace must show pointer movement step by step |
| Greedy | Deep Elaboration: "why is greedy correct?" (proof by contradiction) |

## Software Engineering / System Design

**High-frequency scenarios:**
- Design patterns → Analogy teaching + Generative Learning (have the user come up with use cases themselves)
- System design → Mental Model (draw the architecture diagram first) + Socratic (probe trade-offs)
- Microservices / Distributed → Dual Coding (sequence diagrams + text) + Deep Elaboration
- Database design → Interleaved Practice (mix SQL optimization, index design, transaction isolation)

## AI / LLM Technology

**High-frequency scenarios:**
- Transformer architecture → Mental Model + Dual Coding (attention matrix visualization)
- Training techniques (LoRA, RLHF, etc.) → Simplification + Analogy (build intuition first, then go into the math)
- Prompt Engineering → Interleaved Practice (give different scenarios, have the user write prompts and compare results)
- RAG / Agent architecture → Mental Model (draw the flowchart) + Generative Learning (have the user design a solution)

## Interview Preparation

**Best mode combination: Socratic + Advanced Recall (Mode B)**

The core of interviewing is not memorizing answers — it's being able to reason independently under pressure.

Flow:
1. User names a topic (e.g., "Redis persistence")
2. First have the user state what they already know (reveals the boundaries of their understanding)
3. Simulate an interviewer's follow-up chain (typically 3–5 layers deep)
4. After each answer, note: How good was this answer? Would the interviewer be satisfied?
5. End with a complete knowledge map + list of weak spots

## Other CS Domains

For CS sub-domains not listed above, the default strategy is:
1. Use **Simplification + Analogy** to establish a foundational framework (the 12-year-old version)
2. Use **Mental Model + Dual Coding** to deepen understanding
3. Switch to **Interleaved Practice + Generative Learning** to have the user produce output and self-test
