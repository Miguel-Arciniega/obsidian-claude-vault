# Claude Vault — Operating Spec

Self-sufficient. Do not depend on `Templates/`.

## Hard Rules

1. **Read before write.** Search/read relevant notes before answering vault-relevant questions. Write only at end of conversation.
2. **Wikilinks only.** `[[Note Name]]`. No markdown links, no folder paths.
3. **No secrets.** No credentials, API keys, account numbers, government IDs, full addresses.
4. **Newest stated position wins.** Update the note; do not argue from a stale version.
5. **One canonical Topic per subject.** Append before creating.
6. **Closed tag vocabulary.** No ad-hoc tags.

## Structure

- `Topics/` — canonical reference, one per subject.
- `Conversations/` — session notes, `YYYY-MM-DD Topic Name.md`, `-2`/`-3` on collisions.
- `Indexes/Home.md` — landing page (pure link list).
- `Indexes/Conversations.md` — rolling 6-month log.
- `Indexes/Archive YYYY.md` — older months.

## Read-Before-Write

For questions touching personal routines, research, or preferences:

1. **Dedup pass first** — `search_notes` + `get_frontmatter`. Do not `read_note` yet.
2. Read full content of the 1–2 strongest candidates only.
3. Cite with `[[wikilinks]]`. If vault has nothing, say so — do not fabricate continuity.
4. Skip this protocol entirely for one-off lookups, casual questions, and non-vault topics.

## Save Protocol

### Threshold (save only if at least one applies)

- A decision worth recalling.
- A non-obvious fact hard to re-derive.
- Research with meaningful effort (multiple comparisons, ranked options).
- A stable preference or constraint.

### Do not save

One-off lookups, debugging with no conclusion, secrets, or pure restatements of an existing Topic.

### What to capture

Conclusions and the reasoning behind them. Skip the back-and-forth. Dense dated bullets, not paragraphs.

### Save steps (use efficient tools)

1. **Topic note** — `search_notes` for dedup. If a note covers the subject even partially, append. Otherwise create.
   - Append a new entry: `patch_note` to insert under `## Updates`, newest first, headed `### YYYY-MM-DD — short label`.
   - Supersede prior content: `patch_note` the relevant section in place AND log the change in `## Updates`.
   - Bump `updated:`: `update_frontmatter` (never full-rewrite for a date bump).
2. **Conversation note** — `write_note` (new file, full write is correct here).
3. **Index update** — `patch_note` to insert one bullet under the right `## YYYY-MM` heading in `Indexes/Conversations.md`. Never read+rewrite the whole file.
4. **New Topic only** — `patch_note` to insert the wikilink under the right section in `Indexes/Home.md`.

**Rule:** full-file `write_note` is only correct for (a) creating a new note or (b) restructuring an existing note. Edits to existing notes use `patch_note` or `update_frontmatter`.

## Conflict Resolution

When new info contradicts a Topic note:

1. Treat new info as current truth in the response.
2. `patch_note` the relevant section in place.
3. `patch_note` a `### YYYY-MM-DD — Changed` entry under `## Updates`: what changed, before, after. Never silently delete.

## Schemas

### Frontmatter

```yaml
---
title: "Note Title"
date: YYYY-MM-DD       # creation, never changes
updated: YYYY-MM-DD    # bump on meaningful change
tags: [topic|conversation|index, <subject>]
aliases: []            # Topic only — alternate names for dedup
status: raw            # Conversation only — raw → reviewed → archived
---
```

### Topic note skeleton

```markdown
# Subject

## Summary
One paragraph. Current bottom line.

## Details
Reference content. Subheadings as needed.

## Related
- [[Other Topic]]

## Updates
### YYYY-MM-DD — created
From [[Conversations/YYYY-MM-DD Topic Name]].
```

### Conversation note skeleton

```markdown
# Topic Name

## Summary
2–4 sentences. What was discussed and concluded.

## Details
- Decisions, key points, non-obvious facts. One section, dense bullets.

## Related
- [[Topic Note]]
```

Filename carries the date; `title:` does not repeat it. No `topics:` frontmatter field — `## Related` wikilinks are indexed by Obsidian's backlink graph.

## Tag Vocabulary (closed)

**Structural:** `topic`, `conversation`, `index`.

**Subject (12):** `skincare`, `haircare`, `grooming`, `apparel`, `fragrance`, `nutrition`, `fitness`, `health`, `finance`, `home`, `hardware`, `software`.

Disambiguation: `grooming` = body care distinct from face (deodorant, body wash, shaving, body acne). `health` = sleep, posture, general wellbeing not covered by skincare/nutrition/fitness. `home` includes climate management. `hardware` = physical computing kit. `software` = code, dev tooling, MCP, Obsidian, VMs.

Default to one subject tag. Multiple only when the note genuinely spans subjects. To add a new tag: edit this list in the same commit. Not on the list = does not exist.

## Archival

- `Indexes/Conversations.md` keeps rolling 6 months; older entries `patch_note`-moved to `Indexes/Archive YYYY.md` at month start.
- Topic notes are never archived. Obsolete ones get `status: archived` + a `> [!warning]` callout pointing to the replacement.

## Callouts

`> [!tip|warning|note|info|example|danger] Title`. Use sparingly.
