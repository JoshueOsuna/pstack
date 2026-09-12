# pstack

Working copy of [Lauren Tan](https://x.com/poteto)'s **pstack** Cursor plugin: skills, playbooks, and watchers for running coding agents like a real engineering team — including babysitting PRs, swarming cloud agents, and keeping a reviewable decision trail.

Original plugin: MIT, Copyright (c) 2026 Lauren Tan. Full upstream README: [README.upstream.md](./README.upstream.md).

## Who is Lauren Tan?

She is an engineer. The circulating video mixes two true-but-easy-to-confuse labels:

| Claim you hear | What is actually true |
|---|---|
| "Cursor engineer" | Yes. She was a Member of Technical Staff at Cursor, where pstack started as her internal `/lauren-mode` workflow. |
| "SpaceX engineer" | Close, but not SpaceX the rocket company. She is now a **Principal Engineer at SpaceXAI**, working on **Grok Bot and Cursor**. |
| Same company? | Cursor and SpaceXAI are linked in her current role (she ships Cursor work from SpaceXAI). They are not the same as SpaceX. |

Also true, from her public profiles:

- Cursor profile: [cursor.com/@lauren](https://cursor.com/@lauren) (`@poteto`)
- LinkedIn: [laurenelizabethtan](https://www.linkedin.com/in/laurenelizabethtan)
- React Core team — React Compiler
- Previously Meta (staff engineer / EM) and Netflix
- Open-sourced pstack after the Cursor eng team used her skills ~10,000 times in a week

## What pstack is

Pstack is **not** a dashboard app. It is a Cursor plugin (markdown skills + a `poteto-agent` subagent + scripts). The core command is `/poteto-mode`: it picks a playbook (bug fix, feature, investigation, babysit, swarm, overnight run, …) and routes the rest of the skills for you.

The “monitor agents” piece from the video is built in:

- **Babysit playbook** — own an open PR/stack: CI, review threads, merge-ready. Does not merge unless you ask.
- **`watch-pr`** — GitHub PR watcher used by babysit (`skills/poteto-mode/scripts/watch-pr/`)
- **`/swarm`** — fan out parallel cloud agents and get one report back
- **`/show-me-your-work`** — decision trail during autonomous runs
- **`/setup-pstack`** — map models to roles (code vs judgment vs review panel)

## Official source (download this)

Pstack is a folder inside Cursor’s public plugins repo, not a standalone GitHub project from Lauren:

| What | URL |
|---|---|
| Official plugin | [github.com/cursor/plugins/tree/main/pstack](https://github.com/cursor/plugins/tree/main/pstack) |
| Cursor marketplace | [cursor.com/marketplace/cursor/pstack](https://cursor.com/marketplace/cursor/pstack) |
| Add-plugin PR | [github.com/cursor/plugins/pull/73](https://github.com/cursor/plugins/pull/73) by `@poteto` |
| Community standalone mirror | [github.com/backnotprop/pstack](https://github.com/backnotprop/pstack) |

Clone just pstack from upstream:

```bash
git clone --depth 1 --filter=blob:none --sparse https://github.com/cursor/plugins.git
cd plugins
git sparse-checkout set pstack
```

Or install inside Cursor with no clone:

```text
/add-plugin pstack
```

Then:

```text
/setup-pstack
/poteto-mode <what you want the agent to do>
```

## Your public GitHub copies

Your GitHub account is [JoshueOsuna](https://github.com/JoshueOsuna). You had no public repos yet, so two public copies were created:

1. **Official fork of the whole plugins marketplace** (GitHub fork relationship to `cursor/plugins`):
   - https://github.com/JoshueOsuna/plugins
   - pstack lives at [`plugins/pstack`](https://github.com/JoshueOsuna/plugins/tree/main/pstack)
2. **Dedicated public pstack repo** (this plugin only, for day-to-day use):
   - https://github.com/JoshueOsuna/pstack

Download the dedicated copy:

```bash
git clone https://github.com/JoshueOsuna/pstack.git
```

## Use it in Cursor

Fastest path (marketplace, always current):

```text
/add-plugin pstack
/setup-pstack
/poteto-mode babysit my open PR and keep it merge-ready
```

Local path (this checkout / your GitHub fork):

1. Open this repo in Cursor.
2. If the plugin is not already installed from the marketplace, add it from this folder (Cursor → Plugins, or `/add-plugin` pointing at this directory).
3. Run `/setup-pstack`, then `/poteto-mode`.

Plugin metadata is in [`.cursor-plugin/plugin.json`](./.cursor-plugin/plugin.json) (currently upstream v0.15.2). Skills live in [`skills/`](./skills/). The `poteto-agent` subagent is [`agents/poteto-agent.md`](./agents/poteto-agent.md).

## License

[MIT](./LICENSE) — Copyright (c) 2026 Lauren Tan.
