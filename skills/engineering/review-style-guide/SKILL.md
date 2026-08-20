---
name: review-style-guide
description: Reviews a diff or pull request against the repo's own style guide (CLAUDE.md, CONTRIBUTING.md, or STYLE.md). Use when the user asks to review a PR, diff, or changes against code style/conventions.
---

# Review Style Guide

## Purpose
Check a diff or PR against the style guide already checked into the repo being reviewed, and report violations.

## When to use
- "review this PR against our style guide"
- "does this diff follow our conventions?"
- "check my changes against the style guide"

## When NOT to use
- No style guide file exists in the repo (nothing to check against) — say so instead of guessing at style.

## Instructions
1. Find the style guide in the target repo: look for `CLAUDE.md`, `CONTRIBUTING.md`, or `STYLE.md` at the repo root (in that priority order).
2. Get the diff to review:
   - If given a PR number/URL, fetch it with `gh pr diff <number>`.
   - Otherwise, use the local diff (`git diff` for unstaged, `git diff --staged` for staged, or `git diff main...HEAD` for a branch).
3. Compare each changed file against the relevant rules in the style guide (naming, formatting, patterns, forbidden constructs, etc.).
4. Report findings as a short list: file, line, rule violated, one-line fix suggestion. Skip files/rules with no issues — don't pad the output.

## Notes
- Don't invent style rules that aren't in the guide file.
- If reviewing a GitHub PR, only post comments if the user explicitly asks you to.
