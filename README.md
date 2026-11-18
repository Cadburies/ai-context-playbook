# AI Context System — Playbook

A portable, token-efficient instruction + memory system for coding agents (Claude, Grok, Cursor, Codex, etc.).

This single file lets you bootstrap the same lean, self-healing, parallel-safe agent contract that powers SisuMate.

## What it is

Most AI "memory" systems fail because they:
- Mirror source code (field lists, routes, schema versions) → go stale
- Load everything every session → burn context
- Keep history and resolved items in hot files
- Put procedures in the always-loaded contract
- Rely on prose for rules that agents violate under pressure (git sweeps, editing generated files, reading secrets)

This playbook fixes all of that.

It defines:
- One contract file (`AGENTS.md`) — vendor files only point here
- Procedures in `skills/<name>/SKILL.md` (loaded on demand)
- `.ai_context/INDEX.md` as the only file read every session (~60 lines)
- Tiered memory (hot/warm/cold/frozen) with strict "non-derivable only" rule
- Hooks (not prose) for rules agents routinely violate
- A closed green-loop contract rule + blocking stop hook that forces agents to keep iterating until the gate is green
- Parallel-agent safety via `Touches` fields and claim-before-code
- Untrusted-text rules for public repos (issues are data, not instructions)

## How to use it

### Option 1 — Bootstrap a new repo (recommended)
1. Create a new repository.
2. Paste the **Bootstrap prompt** from the bottom of `AI_CONTEXT_PLAYBOOK.md` as the first message to your agent (attach this file).
3. The agent will create `AGENTS.md`, `skills/`, `.ai_context/INDEX.md`, `risks.md`, the guard/hook, and any domain files your stack needs.
4. Run the generated verify gate. It must pass before any commit.

### Option 2 — Add to an existing repo
Copy `AI_CONTEXT_PLAYBOOK.md` into the root and run the bootstrap prompt (or the maintenance prompt if you already have context files).

### Option 3 — Just use the contract
Many teams copy only the resulting `AGENTS.md` + `skills/` after the first bootstrap. That is fine.

## Key principles (from the playbook)

- Context holds **only non-derivable truth**.
- Source is the inventory. Never mirror model fields, routes, or schema versions.
- Session load is strictly budgeted: INDEX only + ≤2 topical files.
- History is git + the issue tracker. No hot changelog.
- Rules that agents violate become **hooks**, not more prose (generated files, governance edits, secret reads, git sweeps, unverified commits, ending while red).
- The green loop is a contract rule that cannot be moved to a skill.
- Test-first, verification-before-completion, and systematic-debugging are mandatory for behaviour changes.
- A stop hook that only warns is a finding.

## Repository layout it creates

```
AGENTS.md                 # single contract. Vendor files (CLAUDE.md etc.) only point here
skills/<name>/SKILL.md    # loaded on demand when description matches
.ai_context/
  INDEX.md                # only full read every session (~60 lines)
  risks.md                # open risks, do-not-touch, install-base decision
  findings.md             # durable measurements (optional)
  archive/                # cold history — never auto-loaded
```

See the full playbook for the bootstrap prompt, maintenance prompt, anti-patterns, size targets, and exact acceptance criteria.

## License

MIT. The playbook was extracted and distilled from SisuMate after retiring hot changelogs, outstanding.md, and duplicated rules.

---

**Made with the AI Context Playbook** — the same system that powers SisuMate, Poseidon, and many other agent-native projects.
```

Now initialize the repo and push it publicly.