---
name: improve-codebase-architecture
description: Find deepening opportunities in a codebase, informed by the domain language and the recorded decisions in `.claude/specs/`. Use when the user wants to improve architecture, find refactoring opportunities, consolidate tightly-coupled modules, or make a codebase more testable and AI-navigable.
---

# Improve Codebase Architecture

Surface architectural friction and propose **deepening opportunities** — refactors that turn shallow modules into deep ones. The aim is testability and AI-navigability.

## Glossary

Use these terms exactly in every suggestion. Consistent language is the point — don't drift into "component," "service," "API," or "boundary." Full definitions in [LANGUAGE.md](LANGUAGE.md).

- **Module** — anything with an interface and an implementation (function, class, package, slice).
- **Interface** — everything a caller must know to use the module: types, invariants, error modes, ordering, config. Not just the type signature.
- **Implementation** — the code inside.
- **Depth** — leverage at the interface: a lot of behaviour behind a small interface. **Deep** = high leverage. **Shallow** = interface nearly as complex as the implementation.
- **Seam** — where an interface lives; a place behaviour can be altered without editing in place. (Use this, not "boundary.")
- **Adapter** — a concrete thing satisfying an interface at a seam.
- **Leverage** — what callers get from depth.
- **Locality** — what maintainers get from depth: change, bugs, knowledge concentrated in one place.

Key principles (see [LANGUAGE.md](LANGUAGE.md) for the full list):

- **Deletion test**: imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across N callers, it was earning its keep.
- **The interface is the test surface.**
- **One adapter = hypothetical seam. Two adapters = real seam.**

This skill is _informed_ by the project's domain model. The domain language gives names to good seams; ADRs record decisions the skill should not re-litigate.

## Where this project keeps them

This repository has no `CONTEXT.md` and no `docs/adr/`; both live in `.claude/specs/`, and that is
deliberate — traceability belongs to the specs, so a parallel copy would only drift.

| What the skill needs | Where it is here                                                            |
| -------------------- | --------------------------------------------------------------------------- |
| Domain glossary      | `.claude/specs/000-overview.md` §5                                          |
| Project-wide ADRs    | `.claude/specs/000-overview.md` §6 (`ADR-1`…`ADR-7`)                        |
| Scoped ADRs          | Inside the spec of the area, numbered by it (e.g. `ADR-017-3` in the `017`) |
| Known open questions | The "Em aberto" section of the relevant spec                                |

Read `.claude/specs/README.md` first: it indexes every spec and flags the superseded ones.

## Process

### 1. Explore

Read the glossary (`000-overview.md` §5) and the ADRs of the area you're touching first — both
the project-wide table and the ones scoped to that spec.

Then use the Agent tool with `subagent_type=Explore` to walk the codebase. Don't follow rigid heuristics — explore organically and note where you experience friction:

- Where does understanding one concept require bouncing between many small modules?
- Where are modules **shallow** — interface nearly as complex as the implementation?
- Where have pure functions been extracted just for testability, but the real bugs hide in how they're called (no **locality**)?
- Where do tightly-coupled modules leak across their seams?
- Which parts of the codebase are untested, or hard to test through their current interface?

Apply the **deletion test** to anything you suspect is shallow: would deleting it concentrate complexity, or just move it? A "yes, concentrates" is the signal you want.

### 2. Present candidates

Present a numbered list of deepening opportunities. For each candidate:

- **Files** — which files/modules are involved
- **Problem** — why the current architecture is causing friction
- **Solution** — plain English description of what would change
- **Benefits** — explained in terms of locality and leverage, and also in how tests would improve

**Use the glossary's vocabulary for the domain, and [LANGUAGE.md](LANGUAGE.md) vocabulary for the architecture.** The glossary defines "Shipment," so talk about "the Shipment intake module" — not "the FooBarHandler," and not "the Shipment service."

**ADR conflicts**: if a candidate contradicts an existing ADR, only surface it when the friction is real enough to warrant revisiting the ADR. Mark it clearly (e.g. _"contradicts ADR-0007 — but worth reopening because…"_). Don't list every theoretical refactor an ADR forbids.

Do NOT propose interfaces yet. Ask the user: "Which of these would you like to explore?"

### 3. Grilling loop

Once the user picks a candidate, drop into a grilling conversation. Walk the design tree with them — constraints, dependencies, the shape of the deepened module, what sits behind the seam, what tests survive.

Side effects happen inline as decisions crystallize:

- **Naming a deepened module after a concept not in the glossary?** Add the term to
  `.claude/specs/000-overview.md` §5, in the same one-line style as its neighbours.
- **Sharpening a fuzzy term during the conversation?** Update the glossary entry right there.
- **User rejects the candidate with a load-bearing reason?** Offer an ADR, framed as: _"Want me to
  record this as an ADR so future architecture reviews don't re-suggest it?"_ Only offer when the
  reason would actually be needed by a future explorer to avoid re-suggesting the same thing —
  skip ephemeral reasons ("not worth it right now") and self-evident ones. A decision that spans
  the project becomes the next row of `000-overview.md` §6; one scoped to a module goes in that
  module's spec, numbered by it (`ADR-017-4`). Match the columns already there: decision,
  rationale, rejected alternative.
- **Want to explore alternative interfaces for the deepened module?** See [INTERFACE-DESIGN.md](INTERFACE-DESIGN.md).
