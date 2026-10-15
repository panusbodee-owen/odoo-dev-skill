# odoo-dev — Claude Skill for Odoo Development (all versions)

> Claude writes Odoo 16-19 modules that match your version (no leftover attrs, tree or name_get) and handles Thai needs out of the box: baht text, Buddhist year, Tax ID checksum, WHT. Star it if it saves you time.

Helps developers build and maintain custom Odoo modules for versions 16–19 and future releases (20+), including Thai localization needs. Companion skill for QA & documentation: [odoo-qa-docs-skill](https://github.com/panusbodee-owen/odoo-qa-docs-skill).

| Task | How |
|---|---|
| New module | `scripts/scaffold.py` generates a version-correct skeleton: model, list/form views, action, menu, access rights and a unit test |
| Upgrade / port / review | `scripts/check_compat.py` lists deprecated patterns for the target version (`attrs`, `<tree>`, `name_get`, `oe_chatter`, `group_operator`, `detailed_type`, …) with file:line |
| Models, views, security, wizards, cron, QWeb reports, controllers | `references/patterns.md` |
| Tests and debugging | `references/testing.md` (TransactionCase, Form, tours, odoo-bin commands) |
| Thai requirements | `references/l10n-th.md` (baht text, Buddhist year, Thai PDF fonts, Tax ID checksum, branch, WHT) |
| Version differences | `references/versions.md` — one rule table; unknown future versions get `(verify)` tags until confirmed |

## Structure
```
.claude/skills/odoo-dev/
├── SKILL.md              # rules + router (short)
├── references/           # loaded only when needed
│   ├── versions.md
│   ├── patterns.md
│   ├── testing.md
│   └── l10n-th.md
└── scripts/              # Claude runs these instead of writing boilerplate
    ├── scaffold.py
    └── check_compat.py
```

## Installation

**claude.ai / Claude Desktop** — zip the `odoo-dev` folder (the zip must contain `odoo-dev/SKILL.md`), then Settings → Capabilities → Skills → Upload skill.

**Claude Code (all projects)**
```bash
git clone https://github.com/panusbodee-owen/odoo-dev-skill.git
mkdir -p ~/.claude/skills
cp -r odoo-dev-skill/.claude/skills/odoo-dev ~/.claude/skills/
```

**Whole team** — open Claude Code in this repository, or copy the folder into your Odoo addons repository under `.claude/skills/`.

## Usage
```bash
python3 .claude/skills/odoo-dev/scripts/scaffold.py my_request --version 18 --model x.request --depends base,mail --path addons
python3 .claude/skills/odoo-dev/scripts/check_compat.py addons/my_module --version 18
```
Example prompts:
- `Create a module for purchase requests with two-step approval, Odoo 18`
- `Port addons/my_module from 16 to 18`
- `Add a Thai tax invoice PDF with baht text and Buddhist year`
- `Write unit tests for TC-SO-001..005`

## Why it saves tokens
1. Only `SKILL.md` + `versions.md` + one reference are loaded per task.
2. Boilerplate comes from `scaffold.py`; version checks come from `check_compat.py` — Claude runs them instead of generating or reading code.
3. Answers show only changed blocks, not whole files.

## Extending
- New Odoo version: add rows to `references/versions.md` and the matching regex in `RULES` inside `scripts/check_compat.py`.
- Project conventions (module prefix, author, license): add them to the Coding standard section of `SKILL.md`.

## Status
Scaffolds for v16–19 compile and pass their own version check. Generated modules have not yet been installed on a live Odoo server — test on a disposable database first. v19 rules are marked `(verify)`.
