# agent-skills

Shared [Claude Code](https://docs.claude.com/en/docs/claude-code) skills for our team, packaged as an installable plugin.

## Install

Add this repo as a plugin marketplace, then install the plugin:

```
/plugin marketplace add sumitbatwani/agent-skills
/plugin install agent-skills@agent-skills
```

## Layout

```
skills/
  <category>/
    <skill-name>/
      SKILL.md        # required: frontmatter (name, description) + instructions
      scripts/         # optional: helper scripts the skill can invoke
      references/       # optional: supporting docs, examples
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Only [@sumitbatwani](https://github.com/sumitbatwani) can merge to `main`; everyone else contributes via pull request.

## License

MIT
