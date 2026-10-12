# Classive skills

Agent skills for creating learning resources and integrating the [Classive API](https://classive.app/docs).

Install the Classive skill globally so it is available across projects:

```sh
npx skills add classive-hq/skills --skill classive --global
```

Choose your supported AI client in the installer. For a project-only installation, omit `--global`. This installs instructions; configure your own Classive connection separately using the [MCP client guide](https://classive.app/docs/mcp).

The [Classive skill](skills/classive/SKILL.md) routes to focused workflow guides:

- [Connection and discovery](skills/classive/references/connect.md)
- [Clips, lessons and written packs](skills/classive/references/generate.md)
- [Materials and book coverage](skills/classive/references/materials.md)
- [Curricula and selected/full courses](skills/classive/references/curriculum.md)
- [Document reviews](skills/classive/references/review.md)
- [Jobs, cancellation, retry and revision](skills/classive/references/jobs.md)
- [Delivery and learner/instructor packages](skills/classive/references/delivery.md)
- [Application integration](skills/classive/references/integration.md)

Each guide explains the agent's decisions and includes MCP/REST routes. Agents load only the files needed for the task, then fetch current schemas, limits, prices and client configuration from [Classive's Markdown documentation index](https://classive.app/llms.txt). The repository does not duplicate the live API manual.

Installing instructions does not start generation, spend credits or configure credentials. MCP keys and REST keys are separate; uploads/downloads and some controls use REST or the dashboard.
