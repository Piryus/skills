<div align="center">

# 🧰 skills

**A personal collection of agent skills — reusable, self-contained playbooks I hand to Claude.**

*Not your average todo helpers. These are the niche, hard-won procedures I actually reach for.*

[![License: MIT](https://img.shields.io/badge/License-MIT-black.svg?style=flat-square)](LICENSE)
[![Skills](https://img.shields.io/badge/skills-1-brightgreen.svg?style=flat-square)](#-the-collection)
[![Format](https://img.shields.io/badge/format-SKILL.md-blue.svg?style=flat-square)](#-how-a-skill-works)
[![Built for](https://img.shields.io/badge/built%20for-Claude%20Code-d97757.svg?style=flat-square)](https://docs.claude.com/en/docs/claude-code)

</div>

---

## ✨ What is this?

A **skill** is a small folder containing a `SKILL.md` — a focused set of instructions that teaches an AI agent how to do *one thing well*. Drop it where your agent looks for skills and it loads **only when it's relevant**, triggered by the `description` in its frontmatter.

Think of this repo as a toolbelt: each skill is a tool I've sharpened on a real task and saved so I never have to re-explain it. New ones land here whenever I find a procedure worth keeping.

## 🧭 The collection

| Skill | What it does | Reach for it when… |
| --- | --- | --- |
| [**claim-db-compensation**](claim-db-compensation/) | Files a Deutsche Bahn passenger-rights (Fahrgastrechte) claim end-to-end on bahn.de — delay refund **and** reimbursement of an extra seat/ticket. | a DB train was late or cancelled and you want your money back. |

> _More on the way — these aren't limited to everyday life; anything I find myself doing more than once is fair game._

## 🩻 How a skill works

```
claim-db-compensation/
├── SKILL.md        # the playbook the agent loads (name + description + steps)
└── REFERENCE.md    # deeper detail, pulled in only when needed
```

```yaml
---
name: claim-db-compensation
description: Claim DB compensation… Use when the user wants to file a DB delay claim…
---
```

The agent reads every skill's `description`, matches it against what you asked, and loads the full `SKILL.md` only for the one that fits. Keep instructions tight; push the long tail into `REFERENCE.md`.

## 🚀 Use a skill

These follow the [Claude Code](https://docs.claude.com/en/docs/claude-code) / agent-skill convention (skills live in `~/.claude/skills/`).

```bash
# clone the collection
git clone https://github.com/Piryus/skills.git ~/code/skills

# link the skills you want into your agent's skill directory
mkdir -p ~/.claude/skills
ln -s ~/code/skills/claim-db-compensation ~/.claude/skills/claim-db-compensation
```

Then just ask in plain language — e.g. *"claim DB compensation for my last train"* — and the matching skill activates automatically. Prefer a copy over a symlink? `cp -r` works too.

## ➕ Add your own

1. Create a folder named after the skill (`kebab-case`).
2. Add a `SKILL.md` with `name` + a trigger-rich `description` in the frontmatter, then concise steps.
3. Split anything long into `REFERENCE.md` / `EXAMPLES.md`; add a `scripts/` folder for deterministic helpers.
4. Keep `SKILL.md` under ~100 lines and free of time-sensitive details.

## 📜 License

[MIT](LICENSE) — use them, fork them, sharpen your own.
