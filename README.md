# Classive skills

Agent skills for creating learning resources and integrating the [Classive API](https://classive.app/docs).

Install the Classive skill globally so it is available across projects:

```sh
npx skills add classive-hq/skills --skill classive --global
```

Choose your supported AI client in the installer. For a project-only installation, omit `--global`. This installs instructions; configure your own Classive connection separately using the [MCP client guide](https://classive.app/docs/mcp).

The [Classive skill](skills/classive/SKILL.md) covers clips, lessons, study packs, reviews, curriculum approval and course generation. It reads current documentation when needed rather than embedding an outdated API contract.
