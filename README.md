# GrokNotes

**Semantic context for your Obsidian vault.** Discovery. Gap detection. Context bundles. Smart chat.

---

> ⚠️ **Status: Ideation / Pre-Alpha**
>
> GrokNotes is in early concept exploration. Nothing here is built yet. We're working through the design in the open because the problem is worth solving, and we'd rather share the thinking early than wait for a polished release.
>
> Expect rough edges, incomplete ideas, and course corrections. That's what this phase is for.

---

## The Problem

Obsidian is the best tool ever built for deliberate knowledge management. You [[wikilink]] ideas together. You build a graph. You see connections grow over time.

But your vault knows only the links you *already made*. It doesn't know what your notes *mean*. You lose notes not because they're gone, but because you forgot they existed. And every time you want to use AI with your vault, you're back to hunting-and-pasting context.

**Three gaps:**

1. **Discovery** — You can't find notes you can't remember. A semantic similarity layer tells you what your active note is *about*, not just what it's linked to.
2. **Gaps** — The most valuable connections are the ones you *haven't made*. When two notes talk about the same thing but don't link to each other, that's a structural hole worth knowing about.
3. **Context** — Reusable, curated context bundles. Save a set of notes for a project. Inject them into AI with one click. Come back weeks later and pick up where you left off.

---

## The Approach

GrokNotes adds a **semantic layer** alongside your existing wikilink graph. It never replaces it — it enhances it.

| Layer | What it does |
|---|---|
| **Discover** | Notes semantically similar to your active note, with similarity scores |
| **Surface Gaps** | Similar notes that lack a [[wikilink]] — and the option to create one |
| **Bundle** | Named, durable, curated context bundles that persist across sessions |
| **Chat** | Embedded AI chat that receives the current context bundle |

The core insight: **value sits at the intersection of similarity and connectivity.** The most actionable signal is not "these notes are similar" — it's "these notes are similar AND unlinked."

---

## Current Status

- ✅ Repository bootstrapped
- 🔄 PRD in ideation — defining what we're building and why
- ❌ Nothing coded yet

---

## Getting Involved

We're not ready for contributions yet — we don't even know what we're building. But if this resonates:

- **Star the repo** to signal interest
- **Open a Discussion** if you've thought about this problem space
- **Read the [PRD](./PRD.md)** to understand our current thinking

---

## License

MIT — because knowledge tools should be open.
