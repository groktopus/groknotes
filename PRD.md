# GrokNotes — Product Requirements Document

> **Status:** Ideation / Pre-Alpha
> **Owner:** Magnus Hedemark (groktopus)
> **Last Updated:** 2026-05-31

---

## 1. Elevator Pitch

Obsidian is the best knowledge management tool in existence — but its graph is made of links *you already know to make*. GrokNotes adds a semantic layer underneath: it knows what your notes actually *mean*, surfaces connections you've missed, bundles curated context you can reuse, and lets you talk to AI with that context injected — all without leaving your vault.

---

## 2. Problem Statement

Obsidian excels at building knowledge through deliberate connection. But three gaps persist:

**Gap 1 — Discovery is manual and binary.** You can only discover notes you've explicitly [[wikilinked]]. The vault knows every word you've written, but it can't tell you "these 4 notes are about the same concept" unless you already connected them. Semantic similarity — the ability to find notes that *talk about the same thing* even when they never mention each other — requires an external vector layer that Obsidian doesn't ship.

**Gap 2 — You don't know what you're missing.** The most valuable connections are the ones you *haven't made yet*. A note about LoRa modulation limits and a note about MeshCore payload design might both live in your vault, share a subject, and never cross-reference each other. No existing tool surfaces these structural holes. The absence of a link can be as informative as its presence — but only if you have a way to *see the absence*.

**Gap 3 — Context is trapped in notes.** When you want to work with AI on a specific project, you have to manually gather relevant notes, paste them in, and repeat the exercise every session. There's no concept of a *named, reusable, curated context bundle* that persists across sessions, auto-updates as your notes evolve, and can be injected with one click.

---

## 3. Vision

GrokNotes adds a **semantic context layer** to Obsidian that sits alongside — never replaces — the existing wikilink graph. It enables a three-part workflow:

| Layer | What it does | For whom |
|---|---|---|
| **Discover** | Shows notes semantically similar to your active note, ranked by relevance | Anyone finding their own knowledge |
| **Surface Gaps** | Highlights similar notes that *lack* a [[wikilink]] — and offers to create one | Serious knowledge workers building durable connections |
| **Bundle** | Lets you save, name, curate, and reuse sets of notes as project context | Project-based workers, AI users |
| **Chat** | Embedded AI chat that receives the current context bundle | Anyone using AI with their notes |

The core insight: **value increases at the intersection of the semantic and the structural.** Vector similarity tells you what's *about* the same thing. The wikilink graph tells you what's *connected*. The delta between them — highly similar but unlinked — is where the highest-value interventions live.

---

## 4. Target Audience

**Primary:** Obsidian users who maintain a substantial vault (>500 notes) and actively use AI tools (ChatGPT, Claude, local LLMs) for knowledge work — writers, researchers, engineers, students, solopreneurs.

**Secondary:** Anyone who uses Obsidian and has felt the frustration of knowing a note exists somewhere but not being able to find it.

**Not for:** Users with small vaults (<100 notes), users who don't care about cross-note discovery, or users who exclusively use Obsidian for daily journaling with no long-term knowledge graph ambitions.

---

## 5. User Stories

### 5.1 Discovery

- **As a** knowledge worker, **I want to** see notes related to the note I'm currently editing, **so that** I can rediscover relevant knowledge I've already captured.

- **As a** researcher, **I want to** see *why* a note is considered related (which specific passages triggered the similarity), **so that** I can judge relevance without opening every candidate.

- **As a** writer, **I want to** see my vault's embeddings update as I write, **so that** new connections appear in real-time.

### 5.2 Gap Detection

- **As a** Zettelkasten practitioner, **I want to** see a list of notes that are semantically similar to my active note but *not* linked to it, **so that** I can close knowledge gaps by adding [[wikilinks]].

- **As a** project manager, **I want to** see clusters of notes that *should* be connected based on shared themes but aren't, **so that** I can ensure my project knowledge graph is structurally sound.

- **As an** LLM power user, **I want to** select a gap suggestion and have the plugin ask an LLM to generate a synthesis note, **so that** I can bridge disconnected knowledge domains with minimal friction.

### 5.3 Context Bundles

- **As a** project-based worker, **I want to** save a curated set of notes as a named context bundle (e.g., "MeshCore Protocol Work"), **so that** I can return to it across sessions.

- **As a** collaborator, **I want to** export a context bundle to share with a teammate, **so that** they can get up to speed without navigating my entire vault.

- **As an** AI user, **I want to** keep context bundles that auto-update as I add or edit notes, **so that** I'm always injecting current information, not stale snapshots.

### 5.4 Smart Chat

- **As an** AI user, **I want to** start a chat within Obsidian that already has my current context bundle loaded, **so that** I don't have to copy-paste notes.

- **As a** multi-project worker, **I want to** have multiple AI threads linked to different project notes, **so that** each conversation stays scoped to its project.

- **As a** privacy-conscious user, **I want to** see exactly what context the AI can access before sending a message, **so that** I never accidentally expose notes outside my intended scope.

---

## 6. Core Concepts

### 6.1 Semantic Proximity

A measure of how closely two notes relate by *meaning* rather than by explicit link. Computed from the full text of each note, not just titles or tags. Delivered as a similarity score (0–100 or similar) with identifiable source passages.

### 6.2 The Gap Signal

The intersection of two axes: **semantic similarity** (from the vector layer) and **graph connectivity** (from Obsidian's [[wikilink]] graph). The four quadrants:

| | Linked in vault | Unlinked |
|---|---|---|
| **High similarity** | Confirmed connection | ⚠️ **Gap** — should likely be linked |
| **Low similarity** | Cross-domain curiosity | Normal unrelated notes |

The gap quadrant is the primary intervention point.

### 6.3 Named Context Bundles

A durable, named collection of notes curated for a specific purpose (project, theme, client, research thread). Features:
- Created manually by selecting notes or auto-suggested from semantic proximity
- Prunable — remove noise, keep signal
- Versioned or updateable as vault changes
- Exportable (markdown, text, clipboard)
- Injectible into chat or external tools

### 6.4 Chat-Thread-to-Note Binding

An AI conversation thread that is permanently associated with a specific note (and optionally a specific context bundle). Survives vault restarts. Supports multiple threads per note (archived/active). Guarantees the AI always has the relevant context without requiring the user to re-establish scope.

---

## 7. Principles (Design Coherence)

These principles guide every decision about what GrokNotes should and shouldn't do:

1. **Obsidian is the source of truth for the graph.** GrokNotes never manages a second graph. It reads Obsidian's [[wikilink]] structure to determine connectivity. It does not create its own parallel graph ontology.

2. **The human decides.** All suggestions are advisory. GrokNotes surfaces gaps, bundles, and connections — but never auto-creates links or auto-injects context without explicit action. AI synthesis is opt-in per action.

3. **One semantic layer, one substrate.** The vector index is a single, portable dependency. No servers, no daemons, no Docker. The plugin manages its own index and keeps it current.

4. **Privacy by architecture.** The vector index lives in the vault. No data leaves the machine unless the user explicitly copies context to an external AI tool. AI access is always scoped to what the user explicitly provides — never the full vault.

5. **The delta is the value.** The most important information GrokNotes provides is not what's similar, but what's similar-and-not-connected. The gap signal is the differentiator from every other semantic plugin.

6. **Context is a first-class object.** Named context bundles are as important as individual notes. They can be created, saved, named, curated, reused, and shared. They are not merely query results — they are user-owned artifacts.

7. **Works offline first.** The entire core workflow (discovery, gap detection, bundle management) functions without network access. AI features (synthesis, chat) require a configured LLM provider but are additive, not foundational.

---

## 8. Success Criteria

### 8.1 Functional Must-Haves (v0.1)

- [ ] Plugin loads in Obsidian without errors
- [ ] Vector index builds from vault content on install
- [ ] Index updates incrementally on note create/edit/delete
- [ ] Side panel shows semantically similar notes for the active note
- [ ] Side panel indicates which similar notes are linked vs unlinked
- [ ] User can create a named context bundle from selected notes
- [ ] User can inject a context bundle into an AI chat within Obsidian
- [ ] Chat thread persists and stays tied to its note across restarts

### 8.2 Quality Gates

- **Index build time:** < 5 minutes for 10,000 notes (background, non-blocking)
- **Query latency:** < 500ms for similarity lookup on active note
- **Bundle injection:** < 2 seconds for a bundle of 20 notes
- **Memory footprint:** < 200 MB RSS above baseline Obsidian

### 8.3 Non-Goals (explicit out-of-scope for v0.1)

- Multi-vault sync or cloud features
- Real-time collaborative editing
- Mobile support (Obsidian mobile plugin API constraints)
- Custom LLM fine-tuning
- A separate graph visualization (uses Obsidian's existing graph view)
- Import/export to other PKM tools
- Team/shared context bundles

---

## 9. User Experience Sketch

### 9.1 The Discovery Panel (Right Sidebar)

When a note is active, the panel shows:

```
┌─────────────────────────────┐
│  GrokNotes                  │
│                             │
│  ▸ Related Notes (12)       │
│                             │
│    MeshCore Payload Limits  │  92% ● ●
│    LoRa Modulation Tradeoffs│  87% ● ○  [Link?]
│    Core Protocol Spec       │  84% ● ●
│    Antenna Design Notes     │  71% ● ○  [Link?]
│    BLE vs LoRa Comparison   │  65% ● ●
│                             │
│  ● = linked  ○ = unlinked  │
│                             │
│  [Create Bundle] [Chat]     │
└─────────────────────────────┘
```

### 9.2 The Bundle Manager

A dedicated view (modal or tab) for managing context bundles:

```
┌──────────────────────────────────┐
│  GrokNotes Context Bundles       │
│                                  │
│  [New Bundle]                    │
│                                  │
│  ┌──────────────────────────────┐│
│  │ MeshCore Protocol Work       ││
│  │  8 notes · last used 2d ago ││
│  │  [Inject] [Edit] [Export]   ││
│  └──────────────────────────────┘│
│  ┌──────────────────────────────┐│
│  │ Portia Spider Research      ││
│  │  5 notes · last used 1w ago ││
│  │  [Inject] [Edit] [Export]   ││
│  └──────────────────────────────┘│
└──────────────────────────────────┘
```

### 9.3 Smart Chat

An embedded chat panel that receives the current context bundle:

```
┌───────────────────────────────┐
│  💬 LLM Wiki Project          │
│  Context: MeshCore Core (8n)  │
│                               │
│  ┌─────────────────────────┐  │
│  │ Based on my notes, what │  │
│  │ should I prioritize for │  │
│  │ the next protocol       │  │
│  │ revision?               │  │
│  └─────────────────────────┘  │
│                               │
│  [Send]  [Replace Context]    │
└───────────────────────────────┘
```

---

## 10. Open Questions (For the Ideation Phase)

These are deliberately unresolved — to be answered through prototyping and discussion:

1. **Embedding model selection** — What model provides the best semantic accuracy for note-length content (100–5000 words) on consumer hardware? Tradeoff between quality and latency.

2. **Index update strategy** — Full re-index vs incremental on file change? How to handle deleted notes gracefully?

3. **Bundle storage format** — Plain JSON sidecar file within the vault? A sqlite table alongside the vector index? Obsidian metadata cache?

4. **Thread persistence** — How to store chat threads that survive vault restarts? Obsidian's own data storage API vs a sidecar file.

5. **LLM provider abstraction** — OpenAI-compatible API endpoint only, or plugin architecture for multiple providers? What's the minimum viable surface?

6. **Multi-device support** — If the vault is synced (Obsidian Sync, git, iCloud), does the plugin state travel with it gracefully?

7. **The "inbox" question** — Should there be a persistent queue of gap suggestions that accumulates as you work, or only live suggestions for the active note?

---

## 11. Relationship to Existing Ecosystem

| Tool | What it does | How GrokNotes differs |
|---|---|---|
| **Smart Connections** (Wanderloots) | Vector similarity panel | Adds the **gap signal** (similar + unlinked = actionable), plus context bundles as first-class objects, plus embedded chat |
| **Obsidian Graph View** | Native wikilink graph | Adds the **semantic overlay** — notes that *aren't* linked but *should be* |
| **Obsidian Copilot** | AI chat in Obsidian | Adds **scoped context injection** — the AI only sees what you've curated, not whatever note is active |
| **Obsidian AI** (various) | LLM features | Adds the **discovery → bundle → chat** pipeline as an integrated workflow, not isolated features |
| **Dataview** | Query notes by metadata | GrokNotes queries by *meaning*, not by frontmatter fields |
| **Graph Analysis (community)** | Graph metrics | GrokNotes adds a **second signal** (semantic proximity) to compare against graph connectivity |

---

## 12. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Embedding model too slow on large vaults | Medium | High | Background indexing, incremental updates, model choice |
| sqlite-vec incompatibility with Obsidian's Electron runtime | Medium | Critical | Prototype early, fall back to simple flat-file vector store |
| Users don't trust AI suggestions | Medium | Medium | Always show *why* (source passages), always require human confirmation |
| Feature creep / bloated plugin | High | Medium | Strict non-goals list, modular architecture, ship thin |
| Obsidian API changes break plugin | Low | High | Isolate API-dependent code behind thin adapter layer |
| Vector index sync conflicts across devices | Medium | Medium | Support rebuild-from-scratch as first-class recovery path |

---

## 13. Next Steps

1. **Prototype the vector layer** — Can sqlite-vec run inside Obsidian's Electron runtime? Embedding model benchmarking on note-length text.
2. **Build the discovery panel** — Minimal viable panel showing similar notes with link/unlink status.
3. **Implement gap detection** — Cross-reference similarity results against Obsidian's [[wikilink]] graph.
4. **Ship context bundles** — Create, name, curate, inject. The bundle as a first-class artifact.
5. **Wire smart chat** — Embedded chat that accepts a context bundle with one click.
6. **Dogfood** — Use it on the groktopus vault for a week. Find the rough edges.

---

*This document is a living artifact of the ideation phase. It will change as we learn what works and what doesn't. Nothing here is final until it ships.*
