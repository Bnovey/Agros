# Agent Instructions for This Obsidian Vault

You are maintaining a persistent Obsidian markdown wiki.

## Vault layers

- `raw/` contains original sources. Treat these as immutable. Do not edit, rewrite, or delete raw sources.
- `wiki/` contains LLM-generated synthesis pages. You may create and update these.
- `wiki/index.md` is the main catalogue of all wiki pages.
- `wiki/log.md` is an append-only chronological record of all ingests, queries, updates, and lint passes.

## Ingest workflow

When asked to ingest a source:
1. Read the source from `raw/`.
2. Create a source summary page in `wiki/sources/`.
3. Extract key claims, dates, people, concepts, projects, evidence, and uncertainties.
4. Update or create relevant pages in:
   - `wiki/concepts/`
   - `wiki/people/`
   - `wiki/projects/`
   - `wiki/issues/`
   - `wiki/questions/`
5. Add internal Obsidian links using `[[page-name]]`.
6. Preserve uncertainty and disagreement.
7. Add provenance back to the source page.
8. Update `wiki/index.md`.
9. Append an entry to `wiki/log.md`.

## Query workflow

When answering a question:
1. Read `wiki/index.md` first.
2. Identify relevant wiki pages.
3. Read the relevant pages before answering.
4. Use the raw source pages when provenance is needed.
5. If the answer produces reusable synthesis, save it as a new page in `wiki/questions/` or update an existing page.
6. Log the query in `wiki/log.md`.

## Lint workflow

When asked to lint the wiki:
Check for:
- Contradictions between pages.
- Stale claims.
- Missing citations or provenance.
- Duplicate pages.
- Orphan pages with no links.
- Important concepts mentioned but lacking their own page.
- Pages that need updating after a new source.
- Overconfident claims that should be qualified.

Write lint reports in `wiki/reports/`.

## Style rules

- Use concise markdown.
- Use Obsidian links like `[[concept-name]]`.
- Use YAML frontmatter for wiki pages.
- Preserve important caveats.
- Never invent sources.
- Never silently resolve contradictions. Flag them clearly.
- Prefer updating existing pages over creating duplicates.
