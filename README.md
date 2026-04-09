# Claude Vault

A personal Obsidian knowledge base for capturing durable conclusions from conversations with Claude — product research, technical references, routines, and decisions worth recalling.

The vault is designed to be operated by an LLM, not just read by one. Claude reads and writes to it directly via an Obsidian MCP server and follows a strict operating spec defined in `CLAUDE.md`.

## Why this exists

Chat history is lossy. Useful conclusions — a ranked product comparison, a working routine, a decided-on architecture pattern — get buried under thousands of messages and are effectively gone a week later. This vault is the durable layer underneath the chat: Claude searches it before answering personal questions, appends to it when a conversation produces something worth keeping, and treats the newest entry as current truth.

## Structure

```
.
├── CLAUDE.md              # Operating spec — authoritative, read this first
├── Topics/                # Canonical reference notes, one per subject
├── Conversations/         # Dated session notes (YYYY-MM-DD Topic Name.md)
├── Indexes/
│   ├── Home.md            # Landing page, pure link list grouped by area
│   ├── Conversations.md   # Rolling 6-month conversation log
│   └── Archive YYYY.md    # Older conversation entries
└── Templates/             # Obsidian templates (not required by the spec)
```

`Topics/` is the long-lived reference layer — one canonical note per subject, updated in place. `Conversations/` is the append-only history of where each Topic update came from.

## How Claude uses it

The full operating spec lives in `CLAUDE.md` and is the source of truth. Summary of the load-bearing rules:

- Read before write. Claude searches relevant notes before answering anything that touches personal routines, preferences, or prior research. Writes happen at the end of a conversation, not during it.
- Wikilinks only. All intra-vault links use `[[Note Name]]`, never folder paths or markdown links. Obsidian's backlink graph is the index.
- One canonical Topic per subject. New information appends to or supersedes existing Topics — duplicate Topics are not created.
- Newest stated position wins. When new info contradicts an old Topic entry, the Topic is patched in place and the change is logged under `## Updates`. Stale content is never silently deleted.
- Closed tag vocabulary. Tags are restricted to a fixed list defined in `CLAUDE.md`. No ad-hoc tags. Adding a tag means editing the spec.
- Save threshold. Not every conversation gets saved. Saves require a decision worth recalling, a non-obvious fact, meaningful research effort, or a stable preference.

## Note conventions

Topic and Conversation notes follow the frontmatter and skeleton schemas in `CLAUDE.md`. The short version:

- Topic notes have `## Summary`, `## Details`, `## Related`, and an append-only `## Updates` log (newest first, dated).
- Conversation notes are filename-dated, 2–4 sentence summary, dense bullets, link back to the Topic(s) they updated.
- Frontmatter carries `title`, `date` (creation, immutable), `updated` (bumped on meaningful change), `tags`, and on Topics `aliases` for dedup.

## Privacy

- No credentials, API keys, account numbers, government IDs, or full addresses. Enforced by `CLAUDE.md`.
- Personal preferences, routines, and product research are present by design. If you fork this repo, treat that content as yours to scrub before publishing.

## Using it yourself

If you want to run a vault like this:

1. Install Obsidian and clone (or fork) this repo into your vault folder.
2. Install https://github.com/bitbonsai/mcpvault.
3. Point Claude (Desktop, Code, or any MCP-capable client) at the server.
4. Read `CLAUDE.md`. Adapt the tag vocabulary, folder layout, and save threshold to your own use — the spec is opinionated and tuned to one person's workflow, not a generic template.

The structure is portable; the specific Topics are not.
