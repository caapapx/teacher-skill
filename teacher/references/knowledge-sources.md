# Knowledge Source Adapter Protocol

This file defines how the teacher skill integrates different types of knowledge sources. Design goal: let the skill leverage available knowledge sources on any platform while maintaining graceful degradation.

---

## Knowledge Source Types

### Type 1: User-Uploaded Files

The most direct knowledge source. The user provides documents, code, notes, etc. directly in the conversation.

**Handling:**
- Treat file contents as "textbook" — extract teaching content from them first
- For large files, first create a structured summary, then dive into specific sections as needed
- When citing, note the filename and location (e.g., "Based on Chapter 3 of the OS textbook you uploaded...")

**Supported formats:**
- Text files (.md, .txt, .rst)
- Code files (any programming language)
- PDF (if the platform supports it)
- Images (if the platform supports multimodal)

### Type 2: Local Document Library

Documents in the user's working directory, accessible via file system tools.

**Handling:**
- When the user mentions "project docs," "README," or "design docs," proactively search the working directory
- Use Glob / Grep tools to locate relevant files
- Read and use as teaching material

**Typical paths:**
```
project-root/
├── docs/           → project documentation
├── README.md       → project overview
├── ARCHITECTURE.md → architectural design
└── examples/       → example code
```

### Type 3: Internet Search

Fetch up-to-date information via search tools.

**Trigger conditions (search if any apply):**
- User explicitly requests "look up," "search," or "the latest..."
- Content involves version numbers, release dates, API changes, or other time-sensitive information
- An external concept is mentioned in the user's materials but not explained
- Teaching needs a credible source to substantiate a point

**Platform adaptation:**

Different platforms provide different search tools — the skill should auto-detect and use whatever is available:

| Platform | Search Tool | Notes |
|----------|------------|-------|
| Claude Code + Firecrawl MCP | `mcp__firecrawl__firecrawl_search` | Most full-featured: search + scrape |
| Claude Code + WebSearch | `WebSearch` | Built-in search |
| Claude.ai | built-in search | Auto-available |
| Cursor | built-in @web | Triggered via @ command |
| Generic MCP | depends on config | Check for available search-type MCPs |

**Search result handling:**
- Digest search results and weave them into the teaching flow — don't forward raw results
- Cite the source URL or document name
- If search results conflict with the user's materials, guide the user to compare and analyze

### Type 4: RAG / Vector Database

Enterprise-grade knowledge base, integrated via MCP or API.

**Handling:**
- If the environment has a RAG-type MCP configured (e.g., Pinecone, Weaviate, a custom vector store), treat it as an "enterprise textbook library"
- Use keywords relevant to the teaching topic when querying
- When citing, reference the knowledge entry ID or document path

**Integration pattern:**
```
User question → skill extracts keywords → query RAG → retrieve relevant document chunks → teach based on chunks
```

---

## Degradation Strategy

When a knowledge source is unavailable, automatically degrade:

```
User files → Local docs → Internet search → RAG → Model's built-in knowledge
  (highest priority)                                      (last fallback)
```

Skip to the next level when one is unavailable, but inform the user of the current knowledge source:
- "I couldn't find relevant documents — let me search for the latest information..."
- "No search tool is available in the current environment. I'll teach based on my existing knowledge, but I recommend double-checking any time-sensitive information."

---

## Knowledge Source Citation Templates

During teaching, attribute knowledge sources as follows:

- User materials: "Based on [filename] you provided..."
- Search results: "According to the latest information from [source/URL]..."
- RAG knowledge base: "Based on [document title] in the knowledge base..."
- Model knowledge: No special attribution needed, but for potentially outdated info add: "Note: this is based on my training data — I recommend confirming the latest version."

---

## Extending Knowledge Source Support

The community can extend knowledge source support by:

1. Writing a new MCP Server and configuring it in the user's environment
2. Adding `source-<type>.md` in `references/` describing the integration protocol
3. Referencing the new type in the knowledge source management section of SKILL.md

This design ensures the skill itself doesn't need modification — just add reference files to support new knowledge source types.
