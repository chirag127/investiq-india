# Agent Operating Rules (Kilo / Coding Assistant)

## Mission
This repository builds **InvestIQ India**, an independent research-driven publication and product
platform focused on Indian personal finance, investing, and wealth-building. It contains:
- `content/book/` section files (single source of truth for published text)
- `data/facts/` verified fact records
- `src/` web app (Astro)
- `tests/` automated checks

## Core Principles

1. **Accuracy over speed.** A wrong number is worse than a missing number. Every quantitative claim
   must trace to a verified fact record (`data/facts/`) or cited source. Unverified items are recorded
   in `progress/UNVERIFIED.json` before publication.
2. **No duplication.** Each fact, definition, statistic exists in exactly one canonical source file.
   Other locations reference it; never copy text.
3. **Single source of content.** Published long-form prose lives only in `content/book/`.
   Web pages and PDF rendering consume it; they do not duplicate it.
4. **Micro tasks.** Work is decomposed into tasks of roughly 15-60 minutes in `progress/TASKS.json`.
   Update task status as work proceeds. Never "done" a task without its acceptance checks passing.
5. **State on disk.** Conversation memory is unreliable. Persist state in repository files:
   `progress/TASKS.json`, `progress/STATUS.md`, `progress/LOG.md`, `progress/UNVERIFIED.json`.
6. **Commit discipline.** One task (or coherent change) per commit. No empty commits. Commit only
   when explicitly requested. Never commit partial scaffolding that breaks validators.
7. **Run validators.** Before claiming a task complete, run the applicable validators:
   `npm run validate`, `npm test`, `npm run lint` (when available). Record failures honestly.
8. **No fake progress.** Count only words/sections that actually exist on disk and pass checks.
9. **No claiming unverified.** Do not state results, benchmarks, or facts that were not verified by
   tool output or source. If unsure, mark `UNVERIFIED`.

## Naming Conventions

- Tasks: `T001`, `T002`, ... (see `progress/TASKS.json`).
- Decisions: `D-001`, `D-002`, ... (see `DECISIONS.md`).
- Fact IDs: `FACT-<domain>-<nnnn>` e.g. `FACT-TAX-0001`.
- Section IDs: `S-<nnn>` e.g. `S-001`.
- Chapter IDs: `C-<nn>` / `C-<nnn>`.
- Files: lowercase kebab-case; markdown `.md`; Astro `.astro`; tests suffix `.test.*`.
- Front matter keys: snake_case (`section_id`, `chapter_id`, `est_words`).

## Directive Syntax (placeholder)

Directives embedded in source are single-line, bracketed, lowercase:

```
<!-- directive: name key=value key2="value with spaces" -->
```

Examples (not final):
- `<!-- directive: include fact=FACT-TAX-0001 -->`
- `<!-- directive: chart id=equity-returns -->`
- `<!-- directive: crossref section=S-012 -->`

Status: under design (see `research/`). Not yet verified. Do not treat as implemented.

## File Structure (authoritative)

```
content/book/       single-source published prose (.md with front matter)
content/README.md   format notes
data/facts/         verified fact records (JSON/YAML)
data/datasets/      datasets used in analysis
data/charts/        chart specs/data
src/pages/          routes (Astro)
src/components/     reusable components
src/layouts/        layouts
src/styles/         styles
src/lib/            shared utilities
src/calculators/    financial calculators
scripts/            build/validate scripts (Node)
tests/              unit, integration, e2e
book/templates/     LaTeX/typst templates
book/cover/         cover assets
registry/           build registries (generated)
research/           spikes and research notes
progress/           status, tasks, unverified, log, quality reports
public/             static assets
```

## Working Method

1. Read `progress/TASKS.json` and pick the highest-priority `ready` task.
2. Implement the smallest change that satisfies its acceptance criteria.
3. Run validators.
4. Update task `status`, `attempts`, `updated` (ISO date).
5. Update `progress/STATUS.md` if counts changed.
6. Append a line to `progress/LOG.md` for meaningful milestones.

## Micro-task Definition

- Scope: one file or one check or one small refactor; completable in 15-60 minutes.
- Every task has explicit `acceptance` criteria; tasks without acceptance are invalid.
- Never bundle unrelated edits in a single task.

## Validation Gates (must pass before "done")

- Front matter valid (see `scripts/validate-frontmatter.js` when implemented).
- No placeholder strings in published content:
  `TODO`, `TBD`, `Lorem ipsum`, `xxx`, `TODO-ACCURACY` (list may grow).
- Counts (words, sections) computed by scripts, not hand-estimated.
- Cross-references resolve.
- Tests added/updated for code changes.

## Content Rules

- Single source: never duplicate prose between web and PDF paths.
- Citations: every stat carries a fact ID and source URL/date.
- Placeholders forbidden in published content; skeleton files may contain `TODO` only under
  `docs/` or `research/`, never under `content/`.

## Quality / Review

- Reviewer agent (`reviewer` when configured) must run against new prose.
- Quality reports live in `progress/quality-report.md`.
- Any unverified claim becomes an `UNVERIFIED.json` item.

## Security

- Never commit secrets, API keys, or PII.
- `.env` is gitignored; use `.env.example` for documented variables (none yet).

## When Blocked

- Record blocker in task `notes`.
- Do not fabricate progress; do not mark status `done`.
- Ask the user only when an actual decision is required (use the question tool sparingly).

## Prohibited

- Skipping validators to claim completion.
- Creating content that contradicts verified facts.
- Overwriting `.gitattributes` (do not touch).

*Last updated: 2026-10-16 (initial scaffold).*
