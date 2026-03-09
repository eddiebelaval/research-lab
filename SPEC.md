# SPEC.md — What the Research Lab Is Today

> This is the present. Compare against VISION.md to see the roadmap. Compare against BUILDING.md to see how we got here.

---

## System Overview

The Research Lab is a Claude Code-native research platform. It operates as a slash command (`/research`) that orchestrates subagents to evaluate theses through multi-expert panels, adversarial stress-testing, and iterative refinement. All state is persisted to the filesystem. The knowledge base accumulates across sessions.

**Runtime:** Claude Code CLI (requires Opus-tier model for subagent orchestration)
**State:** Markdown + JSON files on disk (no database, no server)
**Orchestration:** `/research` slash command at `~/.claude/commands/research.md`

---

## Current Capabilities

### Core Pipeline
| Action | Command | Status |
|--------|---------|--------|
| Submit thesis | `/research "thesis"` | Working |
| Process queue | `/research run` | Working |
| Process all | `/research run --all` | Working |
| Naysayer loop | `/research naysayer` | Working (1 loop completed) |
| Intake own work | `/research intake <file>` | Working |
| Ingest external | `/research ingest <source>` | Working |
| Review session | `/research review <id>` | Working |
| Compound findings | `/research compound` | Working |
| Status dashboard | `/research status` | Working |
| Refine finding | `/research refine F-NNN` | Built (Mar 9, untested) |
| Auto-refine | `/research refine --auto` | Built (Mar 9, untested) |
| Convergence score | `/research convergence` | Built (Mar 9, baseline calculated) |

### Modules (5)
| Module | Files | Expert Pool | Status |
|--------|-------|-------------|--------|
| `research` | 10 (includes naysayer) | 12 experts + 8 naysayers | Complete, battle-tested |
| `clinical` | 7 | Clinical safety panel | Complete |
| `engineering` | 7 | Technical architecture panel | Complete |
| `trading` | 7 | 12-member investment board | Complete, 1 round run |
| `realestate` | 7 | Real estate deal panel | Complete, 1 round run |

### Wings (3)
| Wing | Module | Experiments | Status |
|------|--------|-------------|--------|
| CaF | research | 6 queued (EXP-CAF-001-006) | Active |
| Parallax | clinical | 5 queued (EXP-PAR-001-005) | Active |
| DeepStack | trading | 11 queued trading briefs | Active |

### Knowledge Base
| Component | Count | Status |
|-----------|-------|--------|
| Findings | 6 (F-000 through F-005) | Most damaged by naysayer loop |
| Open questions | 30 (OQ-001 through OQ-030) | 0 answered |
| Contradictions | 5 (C-001 through C-005) | All OPEN |
| Adversarial challenges | 21 (10 original + 11 from naysayer) | 5 CRITICAL, 6 HIGH |
| External sources | 0 ingested | No external work absorbed yet |
| Convergence score | 12.5 / 100 | Baseline |

---

## Architecture

```
/research command
    |
    v
Route by argument --> SUBMIT | RUN | REFINE | NAYSAYER | INTAKE | INGEST | ...
    |
    v
Phase pipeline (each phase saves to disk before next):
    Setup --> Gather Context --> Select Experts --> Spawn 3 Subagents (parallel)
    --> Save JSONs --> Synthesize --> Update KB --> Generate Artifact --> Auto-Requeue
    |
    v
Knowledge Base (persistent filesystem):
    findings.md | open-questions.md | contradictions.md | convergence.md
    |
    v
Convergence metric (0-100, recalculated after every cycle)
```

### Anti-Session-Death Protocol
Every phase saves to disk before proceeding. Crash recovery:
- During expert evaluation: re-spawn only missing experts from saved JSONs
- During synthesis: re-run from saved expert JSONs
- During KB update: re-run from saved synthesis
- During artifact generation: re-generate from saved synthesis

### File Formats
- **Briefs:** Markdown with structured sections (thesis, key questions, adversarial priorities, scope tier)
- **Expert outputs:** JSON matching module schema (validated on receipt)
- **Synthesis:** Markdown (convergence/divergence analysis, composite grade, delta tracking)
- **Artifacts:** Self-contained HTML (Factory-Inspired design tokens, no external dependencies)
- **State:** JSON (`state/lab-state.json`)

---

## Design Decisions (Current)

| Decision | Choice | Why |
|----------|--------|-----|
| Grading | Median composite, not mean | Prevents strong dimensions from hiding fatal flaws |
| Promotion threshold | A-range (>= 3.5) = Established | High bar for knowledge base entry |
| CRITICAL cap | Any CRITICAL dimension caps grade at C | One fatal flaw fails the whole thesis |
| Expert count | Always 3 | Triangulation minimum; more adds noise without signal |
| Findings persistence | Append-only, never delete | History is evidence; retirement is a finding, not deletion |
| Refinement depth | Max 5 iterations per finding | Prevents diminishing-returns grinding |
| Scope tiers | NARROW / FOCUSED / BROAD | Constraints breed focus (autoresearch lesson) |
| Accept/reject signal | Binary alongside full rubric | Forces clarity; rich data for analysis, simple signal for decisions |

---

## Known Limitations

1. **No empirical execution.** The lab evaluates arguments, not code. Cannot run experiments against testable predictions.
2. **No overnight autonomy.** `/research run --all` requires an active session. No HYDRA scheduling yet.
3. **REFINE loop is untested.** Built Mar 9, baseline convergence calculated, but no finding has been through the loop yet.
4. **No external source ingestion yet.** The ingest pipeline exists but 0 external sources have been processed.
5. **Single-operator.** No multi-user support, no access control, no collaborative editing of the KB.
6. **No source graph visualization.** Finding relationships exist in the data but aren't navigable visually.
7. **No automated literature search.** All external sources must be manually provided.

---

## Dependencies

- Claude Code CLI (with subagent support via Agent tool)
- Git (for version control of the KB)
- A model capable of structured JSON output (Opus-tier recommended)
- `~/.claude/commands/research.md` (the slash command definition)
