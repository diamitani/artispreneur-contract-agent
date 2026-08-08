# Artispreneur Music Contract Agent

An open, harness-agnostic AI agent skill for drafting, reviewing, and
explaining music-business contracts — built for independent artists,
producers, songwriters, managers, and the small businesses around them.

Works in **Cline** (VS Code), **Roo Code**, **Claude** (claude.ai Projects,
Claude Code, Claude Cowork), **Cursor**, **OpenCode/Codex-style CLI agents**,
**ChatGPT Custom GPTs**, or any LLM harness that can read local files. No
vendor lock-in — the instructions don't assume any single tool name.

> **Not legal advice.** This project helps people understand and draft
> contracts. It is not a substitute for a licensed attorney. See
> [`NOTICE.md`](./NOTICE.md) and
> `skills/music-contract-agent/references/05-safety-and-legal-boundaries.md`.

## What's in here

```
artispreneur-contract-agent/
├── SKILL.md                              ← Cline/VSCode skill entry point (copy to root)
├── AGENTS.md                             ← OpenCode/Codex-style entry point
├── soul.md                               ← identity, voice, hard constraints
├── skills/music-contract-agent/          ← the full skill (portable folder)
│   ├── SKILL.md
│   ├── AGENTS.md
│   ├── soul.md
│   └── references/
│       ├── 00-contract-library-index.md  ← routing table for 30 contract types
│       ├── 01–06-*.md                    ← workflow, drafting rules, safety, QA
│       ├── contracts/*.yaml              ← 30 machine-readable contract records
│       ├── templates/*.md                ← 30 human-readable contract templates
│       └── questionnaires/*.md           ← 30 matching intake questionnaires
├── schema/contract.schema.json           ← strict JSON Schema for all YAML records
├── scripts/                              ← Python tooling (validate, export, render)
├── contracts_export.csv                  ← pre-built spreadsheet export (Supabase, Airtable, Sheets)
├── contracts_export.xlsx                 ← pre-built Excel workbook
├── DATABASE_QUICKSTART.md                ← how to load into your database
└── DATABASE_SCHEMA.md                    ← normalized relational schema (PostgreSQL)
```

## Install in your agent harness

### Cline (VS Code)

1. Copy the `skills/music-contract-agent/` folder into your project's skills directory.
2. Or, copy the root `SKILL.md` and `soul.md` into wherever your Cline
   configuration loads skills from.
3. Cline will read the `SKILL.md` frontmatter and trigger on relevant requests
   ("draft a contract", "review this agreement", etc.).

**Minimum install (just the .md files):** copy `skills/music-contract-agent/SKILL.md`,
`soul.md`, and the entire `references/` folder. That's it — no Python, no
YAML, no web app. The agent reads plain Markdown.

### Roo Code

Same as Cline — copy the entire `skills/music-contract-agent/` folder or
the root `SKILL.md` + `soul.md` + `references/` into your workspace.

### Cursor

1. Clone this repo or download it as a ZIP.
2. Point Cursor's agent at the repo root or at
   `skills/music-contract-agent/SKILL.md` as a context file.
3. Cursor reads Markdown natively — no special setup needed.

### Claude (claude.ai Projects)

1. Create a Project in claude.ai.
2. Upload the contents of `skills/music-contract-agent/` into the Project's
   knowledge, or upload the whole repo as a Project's file set.
3. Claude reads `SKILL.md`'s frontmatter and triggers on relevant requests.

### Claude Code / Claude Cowork

Copy or symlink `skills/music-contract-agent/` into wherever your Claude
environment loads skills from (e.g., a `skills/` directory it's configured
to scan).

### ChatGPT Custom GPT

1. Navigate to the ChatGPT custom GPT builder.
2. Under "Knowledge," upload the Markdown files from
   `skills/music-contract-agent/references/` and `SKILL.md`, `soul.md`.
3. Under "Instructions," paste the contents of `SKILL.md` and `soul.md`.
4. The GPT will use these as its system prompt and reference library.

### OpenCode / Codex-style CLI agents

Point the agent at this repo (or copy `skills/music-contract-agent/` into
your project). These harnesses look for `AGENTS.md` by convention — it's a
thin pointer into `SKILL.md` and `soul.md`, written the way these tools
expect.

### Any other agent / bring your own runner

Everything is plain Markdown and YAML. Feed `SKILL.md`, `soul.md`, and the
relevant `references/` files into whatever context-loading mechanism your
system uses. Nothing in here calls a tool by a vendor-specific name.

### Just want the contracts — no AI agent at all

- **Readable templates:** `skills/music-contract-agent/references/templates/*.md` —
  human-readable, fillable contract templates. No AI required.
- **Structured metadata:** `skills/music-contract-agent/references/contracts/*.yaml` —
  machine-readable records with parties, risk flags, and questionnaire routing.
- **Spreadsheet/database:** Use `contracts_export.csv` or `.xlsx` to load all
  30 contracts into Supabase, Airtable, Google Sheets, or any SQL database.
  See `DATABASE_QUICKSTART.md` for step-by-step instructions.

## Validating the library

Before committing any change to a template, questionnaire, or YAML record:

```bash
pip install -r requirements.txt
python3 scripts/validate_library.py
```

This checks: every YAML record validates against
`schema/contract.schema.json`, every referenced template/questionnaire file
exists, there are no orphan files, and the generated index is in sync. CI
runs this on every push and pull request.

If you edit a YAML record, regenerate the human-readable index before
committing:

```bash
python3 scripts/generate_index.py
```

## Adding a new contract type

1. Write the template: `skills/music-contract-agent/references/templates/<slug>.md`.
2. Write the matching questionnaire: `.../references/questionnaires/<slug>-questionnaire.md`.
3. Write the YAML record: `.../references/contracts/<slug>.yaml`, following
   `schema/contract.schema.json` (see any existing record for the shape).
4. Run `python3 scripts/validate_library.py` — fix anything it flags.
5. Run `python3 scripts/generate_index.py` to update the routing table.
6. Open a pull request. CI re-runs the validator automatically.

See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for more detail.

## License

Code, scripts, and schema: [MIT](./LICENSE).

Contract templates and their licensing status: see
[`NOTICE.md`](./NOTICE.md) — **read this before you make the repo public**,
it flags an important provenance question you need to resolve first.