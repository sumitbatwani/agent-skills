# Contributing

## Adding a skill

1. Copy `skills/examples/example-skill/` to `skills/<category>/<your-skill-name>/`.
   - `<category>` is a loose grouping (e.g. `engineering`, `productivity`, `misc`). Ask if unsure where yours fits.
   - `<your-skill-name>` must be kebab-case and match the `name:` field in its `SKILL.md`.
2. Fill in `SKILL.md`. The `description` field is load-bearing — it's what Claude
   uses to decide whether to trigger the skill, so it must state both:
   - **what** the skill does
   - **when** it should trigger (concrete phrases/contexts)
3. Add the new skill's path to the `skills` array in `.claude-plugin/plugin.json`.
4. Test the skill locally (install the plugin from your branch, or drop the
   folder into `~/.claude/skills/`) before opening a PR.
5. Open a PR. One skill per PR.

## Rules

- No secrets, tokens, API keys, or internal URLs in any file (including `scripts/` and `references/`).
- `name` in frontmatter must be unique across the whole repo and match the folder name.
- Don't edit someone else's skill in your PR unless that's the explicit purpose of the PR.
- Only [@sumitbatwani](https://github.com/sumitbatwani) merges to `main`. Everyone else: fork or branch + PR, and wait for review/approval.

## PR checklist

The PR template will ask you to confirm:

- [ ] Skill name and what it does
- [ ] When it should trigger (example prompts)
- [ ] How you tested it
- [ ] Any external dependencies (tools, credentials, APIs)
