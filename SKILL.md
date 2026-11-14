---
name: human-handoff
description: Build an HTML handoff page of the current work for human review (evidence, file paths, per-item comment boxes, copy-all button) and reply with a clickable file:// link. Use when the user says "handoff" or invokes this skill; needs no extra input.
---

# Human handoff

On invocation, build the page from the current session's work right away. Do not ask questions: if scope is unclear, cover everything done in this session.

## Where
`<repo root>/handoffs/<YYYY-MM-DD>-<slug>/index.html` (no repo: `~/handoffs/...`). Put media in `media/` next to it, linked by relative paths, so the folder is self-contained. Update the page in place on later rounds.

## What goes in
1. **Summary**: what was asked, what was done, current state (done / partial / blocked), in 3–5 lines.
2. **One section per item** (feature, fix, card, file group). Each has: what changed and why; **absolute paths** of changed files as `file://` links (see below); evidence actually observed: screenshots/renders, tables of measured numbers, test command + result; how to reproduce or run it.
3. **Not done / not verified**: open risks, assumptions, untested parts.
4. **A comment box per section** plus one for overall, and a fixed "Copy all comments" bar. Start from `template.html` in this skill folder (inline CSS/JS, comments kept in localStorage, clipboard fallback). Set a unique storage key per page.

Rules: only show evidence you actually produced; label inferences. No external CDNs. Images must load from the page folder.

## Links (always full path, clickable, opens in a browser)
Get the absolute path, then:
- WSL (`$WSL_DISTRO_NAME` set): `file://wsl.localhost/$WSL_DISTRO_NAME/<abs path without leading />`
- Linux/macOS: `file://<abs path>`
- Windows: `file:///C:/<path with forward slashes>`

Use the same form for file links inside the page.

## Verify, then reply
If a browser tool is available, open the page and check that images load, there are no console errors, and copying works. Reply with the clickable link on its own line and a 2–4 line summary of what the page covers.
