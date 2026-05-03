# Flywheel

> **In the LLM era, you fix the skill, not the code.**
> A self-compounding workflow toolkit for Claude Code.

---

## The shift

Traditional development treats **code** as the source of truth. Bugs are fixed in code; quality is gated by human code review.

Flywheel treats **skills and plugins** as the source of truth. Code is a downstream artifact, regenerated whenever the skill set is correct enough. When something goes wrong, the fix lands at the skill layer — not the code layer — so the same class of bug never reappears.

Each rotation of the wheel adds mass: the plugins absorb every imperfection back into themselves and grow.

```
        ┌──────────── swarm ────────────┐
        │  autonomous task execution    │
        ▼                               │
   produce code                         │
        │                               │
        ▼                               │
   ┌─ lens ─────────────────────────────┤
   │  adversarial verification          │
   └─ ▼                                 │
   ┌─ postmortem ───────────────────────┤
   │  blame the skill, not the code     │
   └─ ▼                                 │
   ┌─ tend ─────────────────────────────┘
      keep the wheel clean as it grows
```

The faster it spins, the heavier it gets, the harder it is to stop.

---

## The four plugins

| Plugin | Role on the wheel |
|--------|-------------------|
| **[swarm](https://github.com/chunpaiyang/swarm)** | Push — coarse-grained authorization for autonomous, end-to-end task execution. Sub-agents carry context-polluting work; main session reports only on completion. |
| **[lens](https://github.com/chunpaiyang/lens)** | Calibration — epistemic primitives (`/confirm`, `/think`, `/know`, …) that break the single-context reasoning ceiling with adversarial dispatched sub-agents. |
| **[postmortem](https://github.com/chunpaiyang/postmortem)** | Diagnosis — given a transcript + symptom, identifies which skill's declared contract was violated. Blame lives at the skill layer. |
| **[tend](https://github.com/chunpaiyang/tend)** | Maintenance — lints the rule library for structural drift (file size, broken links, orphans, missing sections). Lets the wheel grow without warping. |

---

## When to use which

```
bug spotted ────►  log it as a working job under your scratch dir
                            │
                            ▼
                    is the cause clear?
                  ┌─────yes─┴─no─┐
                  ▼              ▼
            /swarm:oneshot   is it UI/UX?
                              ┌─yes─┴─no─┐
                              ▼          ▼
                         /swarm:pitch  /lens:confirm
                              │          │
                              └────┬─────┘
                                   ▼
                          /swarm:oneshot

skill misbehaved during the run? ──► /postmortem:postmortem
plugin/skill library drifted?    ──► /tend:tend
```

---

## Quickstart

```
/plugin marketplace add chunpaiyang/flywheel
/plugin install swarm@flywheel
/plugin install lens@flywheel
/plugin install tend@flywheel
/plugin install postmortem@flywheel
```

Then try:

```
/swarm:oneshot fix the failing test in src/foo.ts and commit when green
```

If anything misbehaves, run `/postmortem:postmortem` against the session transcript — the blame will land on a specific skill, not on the code. Fix the skill, push, and the next rotation runs cleaner.

---

## Philosophy

- **Speed beats rigor at the moment** — errors are caught by the next rotation, not by upfront review.
- **Skills are versioned; code is regenerated.** Energy goes into improving the wheel, not babysitting individual outputs.
- **Knowledge compounds.** Every imperfection found becomes mass on the wheel — the system is harder to derail tomorrow than it was today.

---

## Layout

This repo is a **meta-marketplace** — it does not contain plugin source. Each plugin lives in its own repo and evolves independently. The four plugins are referenced by GitHub source in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json).
