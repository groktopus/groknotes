# GrokNotes — Product Requirements Document

**Status:** Ideation / Pre-Alpha
**Owner:** Magnus Hedemark (groktopus)
**Last updated:** 2026-05-31

---

## 1. Elevator Pitch

Obsidian's graph shows you what you've deliberately connected. GrokNotes shows you what your notes *actually mean* — and surfaces what you're missing. Semantic discovery. Gap detection. Curated context bundles. AI chat that knows what you're working on. All without leaving your vault.

---

## 2. Opportunity Solution Tree

An Opportunity Solution Tree connects desired outcomes to customer needs, then to solutions. This keeps us from building features before we've verified the problem.

```

DESIRED OUTCOME
   Users maintain higher-value knowledge graphs with less effort,
   and AI interactions are grounded in curated vault context

├─ OPPORTUNITY 1: "I know a note about this exists somewhere,
│   but I can't find it without remembering the exact title"
│   │
│   ├─ Solution A: Semantic similarity panel for the active note
│   │   └─ Test: Can a user find a relevant note they'd forgotten?
│   │
│   └─ Solution B: Full-vault semantic search (beyond Obsidian's
│       existing search which is keyword-only)
│       └─ Test: Does this replace or supplement the existing search?
│
├─ OPPORTUNITY 2: "I don't know what connections I'm missing
│   between notes I've already written"
│   │
│   ├─ Solution C: Gap overlay — show unlinked similar notes
│   │   └─ Test: Do users actually create links when shown the gap?
│   │
│   ├─ Solution D: Periodic "graph health" report
│   │   └─ Test: Weekly email vs in-plugin dashboard?
│   │
│   └─ Solution E: AI-suggested synthesis for orphan concepts
│       └─ Test: Willing to pay token cost for auto-synthesis?
│
├─ OPPORTUNITY 3: "Every time I start a project session,
│   I spend 10 minutes gathering context I've gathered before"
│   │
│   ├─ Solution F: Named, durable, curated context bundles
│   │   └─ Test: Do users reuse bundles across sessions?
│   │
│   ├─ Solution G: Auto-suggested bundles from note clusters
│   │   └─ Test: Are auto-suggestions accurate enough to use?
│   │
│   └─ Solution H: One-click bundle → clipboard / AI inject
│       └─ Test: Does this reduce time-to-first-AI-interaction?
│
└─ OPPORTUNITY 4: "My AI chats are disconnected from my notes —
    I have to re-explain context every time"
    │
    ├─ Solution I: Embedded AI chat with thread-to-note binding
    │   └─ Test: Do users return to the same thread across sessions?
    │
    ├─ Solution J: One-click context injection into the chat
    │   └─ Test: How many clicks to go from note to AI interaction?
    │
    └─ Solution K: Export chat back into vault (synthesis note)
        └─ Test: Do users want AI outputs persisted in their vault?
```

### What this tree tells us

- **Opportunities 1 and 2 are pre-requisites** — you can't bundle context or chat intelligently without knowing what's similar and what's missing
- **Solution C (gap overlay) is our differentiator** — no other Obsidian plugin shows the delta between semantic similarity and graph connectivity
- **Opportunities 3 and 4 share a solution** — bundles are the bridge between discovery and action; chat is a consumer of bundles

---

## 3. Problem Statements

### 3.1 Discovery

A vault of 1,000+ notes accumulates knowledge faster than any human can index. Users know a relevant note exists — they wrote it — but Obsidian's existing search is keyword-only. Semantic similarity (the ability to find notes that *mean* the same thing even when they use different words) requires an external layer. Without it, the vault's value degrades as it grows: more notes means more noise, not more signal.

**Don't build:** "a better search."
**Build:** A discovery mechanism that surfaces what's relevant to what you're currently working on, ranked by meaning, not keyword match.

### 3.2 Gap Detection

The most valuable connections in a knowledge graph are the ones that don't exist yet. When two notes share a subject but lack a [[wikilink]], that's a structural hole — a missed opportunity for synthesis. Obsidian's graph view shows you only what *is* connected. It cannot show you what *should be* connected. The absence of a link is invisible unless you know to look for it.

**Don't build:** "a link recommender."
**Build:** A signal that compares semantic proximity against actual connectivity and surfaces the delta — with an action to close it.

### 3.3 Context Bundles

Project-based knowledge work requires gathering, curating, and reusing the same sets of notes across multiple sessions. Currently this is manual: open relevant notes, copy contents, paste into AI or share with a collaborator. The act of curating is valuable — it forces you to be selective — but the result is ephemeral. There's no durable artifact that says "these 8 notes are the context for *this project*" that persists across days and weeks.

**Don't build:** "saved searches."
**Build:** A first-class artifact — named, curated, prunable, exportable context bundles that you create once and inject anywhere.

### 3.4 Smart Chat

AI chat threads are disconnected from the knowledge they reference. A conversation about the MeshCore protocol revision lives in ChatGPT's history; the notes it references live in Obsidian. Re-establishing context on return means re-gathering and re-pasting. Thread-to-note binding solves this permanently — the chat knows what it's about because it's linked to the note that defines the project.

**Don't build:** "AI in Obsidian."
**Build:** A chat surface where context is automatically scoped to what the user has curated, and the thread's identity is the project note it's attached to.

---

## 4. Success Metrics

| Layer | Primary metric | Target | Qualitative signal |
|---|---|---|---|
| **Discovery** | Notes discovered per session via semantic panel | 3+ notes/session | "I forgot I had that note" |
| **Gap Detection** | Gaps converted to links per week | 5+/week | "These shouldn't have been disconnected" |
| **Context Bundles** | Bundles created per user in first 30 days | 3+ | "I keep coming back to the same bundle" |
| **Smart Chat** | Chat sessions per week with injected bundle | 5+ | "I didn't leave Obsidian all afternoon" |

**Countermetric to watch for all layers:** Time spent managing GrokNotes exceeds time saved. If the plugin creates more overhead than it eliminates, we failed.

---

## 5. Scope

### 5.1 What We're Building

| Layer | Description |
|---|---|
| **Vector Index** | A semantic index of the vault that maps notes by meaning, not keywords |
| **Discovery Panel** | Right-sidebar view showing similar notes for the active note, with similarity scores and source passages |
| **Gap Overlay** | Visual indicator of which similar notes are linked vs unlinked, with one-click link creation |
| **Context Bundles** | Named, durable collections of notes — create, curate, prune, name, inject |
| **Smart Chat** | Embedded chat panel with thread-to-note binding and one-click context bundle injection |

### 5.2 What We're NOT Building

These are explicit boundaries, not future aspirations. They protect the scope of v0.1.

- **No second graph.** We read Obsidian's [[wikilink]] structure. We don't maintain a parallel graph ontology. The vector index is similarity, not structure.
- **No server or cloud.** Everything runs locally. The vector index is a local file. No accounts, no sync, no SaaS.
- **No auto-linking.** Suggestions only. The human decides what to connect.
- **No full-vault AI access.** AI receives only what the user explicitly provides via context bundles.
- **No mobile support** in v0.1. Obsidian mobile plugin API constraints defer this.
- **No custom embedding model training.** We use existing off-the-shelf models.
- **No multi-vault features.** The plugin operates on a single vault.
- **No real-time collaboration.**
- **No separate graph visualization** — the existing Obsidian graph view is sufficient.

---

## 6. Edge Cases

| Edge case | Status |
|---|---|
| New note created — index doesn't know about it yet | Background re-index triggers on file create; panel shows "indexing" state |
| Note deleted — stale vector entry | Index cleanup on file delete; periodic full compaction |
| Note renamed — vectors stale | Index update on file rename (Obsidian provides rename event) |
| Vault has no [[wikilinks]] at all | Gap detection degrades gracefully to "all notes are discovery" mode |
| Vault exceeds 50,000 notes | Index build time is the constraint; progress bar in status bar |
| Obsidian vault is on a synced filesystem (git, iCloud, Obsidian Sync) | Index travels with vault if stored in vault dir; rebuild-from-scratch as recovery path |
| User has multiple vaults | One index per vault; plugin instance per window |
| Embedding model fails to load (OOM, missing dependency) | Plugin loads without vector layer; clear error message; fallback panels |
| Context bundle references a deleted note | Bundle shows "missing" state; user can remove from bundle |
| Chat thread saved before plugin update | Thread format versioning; migration path on load |

---

## 7. Open Questions

These are explicitly unresolved — to be answered through prototyping and research.

- [ ] What embedding model provides the best quality-to-latency ratio on consumer hardware for note-length content (200–5000 words)?
- [ ] Should the vector index be stored inside the vault directory (portable across sync) or in Obsidian's plugin data directory (isolated)?
- [ ] How does incremental indexing handle binary files, images, and excluded folders?
- [ ] What's the minimum similarity threshold for showing a note as "related" before it becomes noise?
- [ ] Should context bundles support nesting? (A bundle within a bundle?)
- [ ] How many chat threads per note before the UI becomes unwieldy?
- [ ] Should bundles be exportable to a shareable format (Markdown, text) for non-Obsidian users?
- [ ] Does the gap signal benefit from a "gap severity" ranking, or is a binary linked/unlinked sufficient?

---

## 8. Deferred Decisions

| Decision | Why deferred | When to revisit |
|---|---|---|
| Embedding model selection | Multiple viable options; needs prototyping on real vaults | After first prototype with baseline model |
| Vector index storage location | Implications for sync and portability need real-world testing | After first prototype |
| Bundle auto-suggest from note clusters | Value proposition unclear without usage data | After 10+ manual bundles created by dogfood users |
| Thread persistence format | Obsidian's data storage API or sidecar file? | During smart chat implementation |
| Multi-LLM provider support | OpenAI-compatible API first; plugin architecture later | After smart chat ships |
| Bundle export format | Markdown + frontmatter or plain text? | First user request for sharing |

---

## 9. Decision Log

### 9.1 Don't force Cashew into Obsidian

**Date:** 2026-05-31
**Status:** Accepted

**Context:** We maintain the Cashew thought graph (224K nodes, 1.2M edges) as a Hermes memory substrate. Initial instinct was to replicate this inside Obsidian as the plugin's graph layer. But Obsidian already owns the wikilink graph — it's the canonical source of truth for explicit connections between notes. Importing Cashew's typed-edge model (observations, insights, cross-links) alongside Obsidian's existing graph would create two sources of truth for connectivity, with no clear reconciliation path.

**Options considered:**

| Option | Pros | Cons |
|---|---|---|
| A: Embed Cashew in the plugin | Richer graph (typed edges, insights) | Dual truth, Obsidian drift, dependency overhead |
| B: Read-only Cashew sync | Leverage existing graph without conflict | Requires running Hermes; breaks offline assumption |
| C: **Vector-only. No second graph.** (chosen) | Clean architecture, one truth, portable | No typed edges — just similarity scores |

**Rationale:** The value we bring is not a better graph — it's a *semantic overlay* that the existing graph doesn't have. Vector similarity is additive, not conflicting. Adding a second graph would create reconciliation problems that add complexity without clear user benefit.

**Expected outcome:** The plugin discovers notes by meaning, uses Obsidian's [[wikilink]] graph to determine connectivity, and surfaces the delta. No graph conflicts. Users see one graph (Obsidian's) with one enhancement (the semantic layer).

**Signal to reconsider:** Users consistently ask for typed edges or "why" explanations that pure vector similarity can't provide.

---

### 9.2 The gap signal is our differentiator

**Date:** 2026-05-31
**Status:** Accepted

**Context:** Smart Connections (Wanderloots) already provides semantic similarity in Obsidian. If GrokNotes only replicates that, it brings nothing new. The unique value proposition must be something Smart Connections doesn't do.

**Options considered:**

| Option | Pros | Cons |
|---|---|---|
| A: Better similarity (faster, more accurate) | Incremental improvement | Mooré's law catches up; not defensible |
| B: **Cross-reference similarity against graph connectivity** (chosen) | Truly unique signal; no other plugin does this | Requires both vector index AND graph read |
| C: Knowledge graph export | Valuable for research users | Narrower audience |

**Rationale:** The intersection of two independent signals — "what's similar" (vector) and "what's connected" (graph) — produces a quadrant that no other tool surfaces. The gap quadrant (high similarity + unlinked) is actionable, specific, and unique. It's not a better version of an existing feature; it's a new category of signal.

**Expected outcome:** Users report discovering connections they genuinely didn't know were missing — not just "here are some related notes I already knew about."

**Signal to reconsider:** Users only use the discovery panel and ignore the gap indicators entirely.

---

### 9.3 Context bundles are first-class artifacts

**Date:** 2026-05-31
**Status:** Accepted

**Context:** The natural end-state of the discovery → gap → bundle pipeline is a reusable asset. In the Wanderloots video, context bundles were demonstrably the most powerful part of the workflow — a durable named set that persists, updates, and can be injected with one click. Without bundles, every discovery session starts from zero.

**Options considered:**

| Option | Pros | Cons |
|---|---|---|
| A: **Named, curated, persistent bundles** (chosen) | Most powerful for project workers | Requires bundle management UI (scope) |
| B: Ephemeral selection (select notes, use once, discard) | Less UI surface | No reusability; each session starts fresh |
| C: Only auto-suggested bundles (no manual curation) | Zero effort for user | Trust issues — auto-suggested bundles may miss or include wrong notes |

**Rationale:** Bundles are the artifact that makes the workflow durable. A bundle you curated last week and injected today is worth more than a fresh query you run every session. Manual curation is non-negotiable — the act of pruning creates trust in the context.

**Expected outcome:** Users build a small library of context bundles (3-10) that they reuse across sessions. The bundle becomes as important as any individual note.

**Signal to reconsider:** Users create bundles once and never update them.

---

## 10. Prioritization (RICE)

| Layer | Reach (users/q) | Impact | Confidence | Effort (weeks) | RICE Score |
|---|---|---|---|---|---|
| **Discovery panel** | 10,000 | 2 (High) | 80% | 2 | 8,000 |
| **Gap overlay** | 5,000 | 3 (Massive) | 70% | 2 | 5,250 |
| **Context bundles** | 3,000 | 2 (High) | 60% | 3 | 1,200 |
| **Smart chat** | 2,000 | 2 (High) | 50% | 4 | 500 |

**Analysis:**

- **Ship in order: Discovery → Gaps → Bundles → Chat.** Each layer feeds the next. Discovery is the foundation. Gaps layer on top by cross-referencing against the graph. Bundles consume discovery results. Chat consumes bundles.
- **Discovery ships first** because it's highest reach, highest confidence, lowest effort, and is a pre-requisite for everything else.
- **Gaps ships second** because it's our unique value proposition and builds directly on the discovery layer with zero additional infrastructure.
- **Bundles and Chat are higher risk** (lower confidence) and depend on usage patterns we can't predict until Discovery and Gaps are in users' hands. Deferring them doesn't block value delivery.

---

## 11. Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Embedding model incompatible with Obsidian's Electron runtime | Medium | Critical | Prototype sqlite-vec in Electron early; have flat-file fallback |
| Index build time unacceptable on large vaults | Medium | High | Background incremental indexing; progress indicators |
| Users don't trust the gap signal ("these aren't actually related") | Medium | Medium | Show source passages for similarity; let users override suggestions |
| Feature creep from "one more thing" requests | High | Medium | Strict non-goals list; each layer is a discrete shippable unit |
| Dual-source-of-truth creep (plugin starts caching graph state) | Low | High | Design principle explicitly forbids it; enforce in architecture |
| Smart Connections already occupies the "semantic Obsidian" mindshare | Medium | Medium | Differentiate on gap signal, not similarity quality. Don't compete on the same axis. |
| Vector index conflicts on synced vaults | Medium | Low | Rebuild-from-scratch is always available; index is a cache, not source of truth |

---

## 12. Principles (Design Coherence)

1. **Obsidian is the source of truth for the graph.** The plugin reads [[wikilinks]] to determine connectivity. It never creates a second graph.

2. **The human decides.** All suggestions are advisory. No auto-linking, no auto-injection. AI synthesis is opt-in per action.

3. **One semantic layer, one substrate.** The vector index is a single portable dependency. No servers. No daemons.

4. **Privacy by architecture.** The index lives in the vault. AI access is scoped to what the user explicitly provides — never the full vault.

5. **The delta is the value.** The gap signal (similar + unlinked) is the primary differentiator from every other semantic plugin.

6. **Context is a first-class object.** Named bundles are as important as individual notes. They can be created, saved, curated, reused, and shared.

7. **Works offline first.** Core workflow (discovery, gaps, bundles) functions without network. AI features are additive, not foundational.

---

## 13. Relationship to Existing Ecosystem

| Tool | What it does | How GrokNotes differs |
|---|---|---|
| **Smart Connections** | Vector similarity panel | Adds the gap signal (similar + unlinked = actionable), plus bundles, plus chat |
| **Obsidian Graph View** | Native wikilink graph | Adds semantic overlay — what *should* be connected but isn't |
| **Obsidian Copilot** | AI chat in Obsidian | Adds scoped context injection via bundles |
| **Dataview** | Query by frontmatter | Queries by *meaning*, not metadata fields |
| **Various AI plugins** | Varying LLM integrations | Adds the discovery → bundle → chat pipeline as an integrated workflow |

---

## 14. Next Steps

| Step | Deliverable | Depends on |
|---|---|---|
| 1. Validate sqlite-vec in Electron | Confirmed working or fallback plan | Nothing |
| 2. Embedding model bakeoff | Recommendation with latency/quality data | Step 1 |
| 3. Prototype discovery panel | Working panel showing similar notes with scores | Step 2 |
| 4. Implement gap detection | Panel shows linked/unlinked status per result | Step 3 |
| 5. Ship v0.1-alpha | Discovery + Gaps, no bundles, no chat | Step 4 |
| 6. Prototype context bundles | Create, name, curate, inject | Step 5 |
| 7. Prototype smart chat | Embedded chat + thread binding | Step 6 |
| 8. Dogfood for 2 weeks | Usage data, bug reports, UX feedback | Step 5 |
| 9. Ship v0.1-beta | All four layers | Step 7 + Step 8 |
| 10. Public release | Plugin listed in Obsidian community | Step 9 |

---

*This document is a living artifact of the ideation phase. Decisions here will change as we learn. Anything not explicitly decided is intentionally open.*

---

## Appendix A: Key Decisions Log (Summary)

| # | Decision | Rationale | Date |
|---|---|---|---|
| 001 | Vector-only. No second graph. | Obsidian owns the graph; vector similarity is additive | 2026-05-31 |
| 002 | Gap signal is the differentiator | Similarity + connectivity delta is unique and actionable | 2026-05-31 |
| 003 | Bundles are first-class artifacts | Curated, named, durable — not ephemeral selections | 2026-05-31 |
| 004 | Ship order: Discovery → Gaps → Bundles → Chat | Each layer feeds the next | 2026-05-31 |
