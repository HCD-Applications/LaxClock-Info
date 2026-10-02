---
name: changelog-update
description: Fetch the latest GitHub release of HCD-Applications/LaxClock and add it to _pages/changelog.md as the new "Latest Release" entry. Use when the user asks to update the changelog with the latest LaxClock release. Never commits.
---

# Update changelog from the latest LaxClock release

Adds the most recent release of `HCD-Applications/LaxClock` to `_pages/changelog.md`
in this repo. **Do not commit, stage, or push anything** — the user reviews the
change before publishing.

## Steps

1. **Fetch the latest release:**

   ```bash
   gh release view -R HCD-Applications/LaxClock --json name,tagName,body,isDraft,isPrerelease
   ```

   If `gh` fails (not authenticated, etc.), fall back to the GitHub MCP
   `get_latest_release` tool (owner `HCD-Applications`, repo `LaxClock`).
   Version = release `name` (e.g. `1.5.0`); if empty, use `tagName` without the leading `v`.

2. **Check it isn't already there.** Read `_pages/changelog.md`. If it already
   contains `# **Version <version>**`, tell the user the changelog is up to date
   and stop.

3. **Build the entry** from the release body:
   - Strip trailing whitespace from every line and trailing blank lines.
   - If the body already starts with a `# **Version ...**` heading, drop that line
     (it is added below).
   - Remove the "Behind The Scenes" section entirely (any `###` heading starting
     with "Behind The Scenes", case-insensitive, plus all of its lines up to the
     next heading or the end of the body). It is internal and not for the public
     changelog.
   - Keep everything else verbatim — the intro paragraph and the remaining `###`
     sections (Enhancements, Bug Fixes, New Features, …) already match the
     changelog style. Do not reword the notes.

   Entry format:

   ```markdown
   # **Version <version>**
   <body>

   <br>

   ```

4. **Insert it** directly below the `` ### `Latest Release` `` line, so the
   previous latest version moves down beneath the new one. Each entry is separated
   from the next by a blank line, `<br>`, and a blank line. Leave the front matter,
   the `` ### `Initial Release` `` section, and the commented-out block at the bottom
   untouched.

5. **Report**: show `git diff _pages/changelog.md` and remind the user nothing has
   been committed.
