---
name: generate-changelog
description: Generates a changelog section from git commit history, either since the last tag/release or between two refs (branches, tags, or commit SHAs). Groups changes by conventional-commit type (feat/fix/chore/etc.) when commits follow that convention, or by inferred category otherwise, and outputs Keep-a-Changelog-style Markdown ready to paste into CHANGELOG.md. Use whenever the user asks to "generate a changelog," "write release notes," "summarize commits since the last release/tag," or "what changed between <ref> and <ref>" — even if they don't use the word "changelog" explicitly.
---

# Generate Changelog

## Purpose
Turn a range of git commits into a changelog section grouped by type of change, without inventing anything not actually stated in the commit history.

## When to use
- "generate a changelog for the next release"
- "write release notes since v1.2.0"
- "what changed between main and the release branch?"
- "summarize the commits since our last tag"

## When NOT to use
- The user wants a PR-level code review or diff review — use `review-style-guide` instead.
- There are no commits in the requested range — say so; don't pad the output with filler.

## Instructions

1. **Determine the range.**
   - "since the last release/tag": run `git describe --tags --abbrev=0` to find the latest tag, then use `<tag>..HEAD`.
   - Explicit refs given (branches, tags, SHAs): use `<ref-a>..<ref-b>` as given.
   - No tags exist yet: fall back to the full history (`git log`) and say you did so.

2. **Pull the raw commit data**, one commit per line, with enough detail to categorize honestly:
   ```
   git log <range> --no-merges --pretty=format:'%h %s'
   ```
   Skip merge commits — they rarely describe a user-facing change themselves.

3. **Detect whether commits follow [Conventional Commits](https://www.conventionalcommits.org)** (`type(scope): subject`, e.g. `feat(auth): add refresh token rotation`). Check a sample of the messages, not just the first one — a repo may be inconsistent.
   - **If mostly conventional:** group by type. Map types to Keep a Changelog headings:
     - `feat` → Added
     - `fix` → Fixed
     - `perf`, `refactor` → Changed
     - `docs`, `chore`, `test`, `build`, `ci` → put under a lower-priority "Other" section, or omit if the user only wants user-facing changes — ask if unsure.
     - `revert` → note it removes/undoes a prior entry; don't list it as a new addition.
   - **If not conventional (or mixed):** read each commit subject and infer a category (Added/Changed/Fixed/Removed) from what it actually says. Don't force a category onto a message that doesn't support it — see step 4.

4. **Flag commits you can't confidently categorize** rather than guessing. A message like `wip`, `fix stuff`, `updates`, or a bare ticket number gives you nothing to categorize from — list these separately under an "Uncommitted / needs review" note with their short SHA, so the user can either expand on them or decide to drop them. Never invent a plausible-sounding description for a vague commit.

5. **Write the output** in Keep a Changelog format, ready to paste into `CHANGELOG.md`:
   ```markdown
   ## [Unreleased] (or a version, if the user gave one)

   ### Added
   - ...

   ### Changed
   - ...

   ### Fixed
   - ...
   ```
   - One bullet per commit (don't merge unrelated commits into one bullet).
   - Strip the conventional-commit type/scope prefix from the bullet text itself — the heading already conveys that.
   - Keep each bullet close to the original commit subject; light rewording for grammar/clarity is fine, adding claims the commit didn't make is not.

## Examples

**Input:** "generate a changelog since v2.3.0"
**Expected behavior:** run `git describe --tags --abbrev=0` → `v2.3.0`, log `v2.3.0..HEAD --no-merges`, detect conventional-commit style, group into Added/Changed/Fixed, flag any vague commits separately, output Markdown.

**Input:** "what changed between `staging` and `main`?"
**Expected behavior:** log `staging..main --no-merges`, same grouping/output, note explicitly that this is a comparison between branches (not a release) if the user seems to want that distinction called out.

## Notes
- Requires a local git checkout of the repo in question with the relevant refs/tags fetched.
- If the range spans hundreds of commits, say so and ask whether the user wants a full list or a curated/summarized set — don't silently truncate.
- This skill only reads from git history — it does not read the diff content itself, so it can't catch changes the commit message misrepresents. If commit hygiene is poor, the output will reflect that (via the "needs review" flag), not paper over it.
