# Get started with MDC

**MDC (Markdown Checklists) is a checklist format that's always valid Markdown, and also a task graph your tools and AI agents can read and safely edit.** This repo is a 60-second tour. Full project: **[github.com/mdcspec/mdc](https://github.com/mdcspec/mdc)** · spec: **[mdcspec.dev/spec/v0.1](https://mdcspec.dev/spec/v0.1)**.

## 1. It already renders (no tools needed)

Open **[`board.mdc.md`](board.mdc.md)** right here on GitHub. It shows as a normal checklist with real checkboxes, headings, and struck-through cancelled items. That's the point: an MDC file is just GitHub-Flavored Markdown, so it renders everywhere (GitHub, GitLab, VS Code, Obsidian) with zero tooling. The extra bits in `{…}` (ids, `@assignees`, `needs=`) are plain text to anything that doesn't understand them.

## 2. Drive it from the CLI

The `mdc` CLI turns that same file into a queryable, editable task graph. (Not on npm yet, so grab the reference implementation:)

```bash
git clone https://github.com/mdcspec/mdc && (cd mdc && npm install)
alias mdc="node $(pwd)/mdc/packages/mdc/src/cli.js"   # a short command for this shell

mdc next   board.mdc.md          # what's actionable right now (respects needs= and gates)
mdc claim  board.mdc.md form --as me    # take an item (atomic; fails if already taken)
mdc check  board.mdc.md form      # mark it done: a one-line diff you can commit
mdc report board.mdc.md           # a standup view: done / in progress / ready / blocked
mdc add    board.mdc.md "Rate-limit the API" --needs form   # record new work
```

Every change rewrites exactly one line, so edits commit as clean one-line diffs and multiple people (or agents) editing different items merge without conflict. Exit codes are the API: `0` ok, `1` usage/parse error, `2` refused (e.g. the item's already claimed).

## 3. Hand it to your coding agent

This repo ships an **[`AGENTS.md`](AGENTS.md)**, the drop-in snippet that teaches a coding agent (Claude Code, Cursor, Codex, …) to drive MDC files: `claim` before working, `check` after, `add`/`note` as it discovers things. Agents coordinate through the file, and git history is the audit trail. There's also a native **MCP server** (`@mdcspec/mdc-mcp` in the main repo) that exposes every verb as an MCP tool, so an agent host can drive MDC with no shell at all.

## 4. Use it in your own project

1. Copy `board.mdc.md` into your repo (name it anything, e.g. `TODO.mdc.md` or `docs/release.mdc.md`). The only requirement is the `mdc: "0.1"` frontmatter key.
2. Drive it from the CLI or CI; paste this repo's `AGENTS.md` into yours to bring agents along.
3. Reusable process (a release, an incident runbook)? Make it a `kind: template` and `mdc cut` a fresh **run** each time. See the [spec](https://mdcspec.dev/spec/v0.1).

## The syntax in ten seconds

```markdown
- [ ] open item          {#id @assignee needs=other-id due=2026-10-01}
- [x] done item          {#id done=2026-09-10}
- [x] ~~cancelled item~~ {#id reason="why it was skipped"}
```

- `{#id}`: a stable handle (only needed when something references it).
- `@assignee`: who owns it. `needs=a,b`: dependencies. `.gate`: blocks everything after it until done. `.doing`: in progress.
- A bare `- [ ] thing` with no `{…}` is already a complete MDC item. Everything else is opt-in.

---

MDC is a registered `text/markdown` variant (`variant=mdc`, IANA Markdown Variants registry) with an open spec and conformance corpus. Code is MIT, the spec is CC BY 4.0; independent implementations welcome.
