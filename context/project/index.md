# Project context — building winter, with winter

This workspace develops the **winter** framework and its extensions. Each project repo is itself a winter component: the
workspace framework and Python CLI in `winter-workspace` (GitHub: `paul-gross/winter`), the platform launcher repo in
`winter` (GitHub: `c5xlabs/winter`), the conventions repo in `winter-context`, tmux and docker service orchestration in
`winter-service-tmux` and `winter-service-docker`, the agentic workflow in `winter-workflow`, GitHub issue tooling in
`winter-github`, product backlog tooling in `winter-product`, and the public documentation site in `winter-docs`.
Planning governance lives in `winter-product-requirements` and `winter-engineering-requirements` (merged into
`winter-context-planning`, which replaces them), suite product planning in `winter-planning` (platform, workspace, and
fleet projects, migrating in by hand), and Fleet product planning in `blizzard-product` until it moves there.

## Project-level conventions

| Topic                                             | Where to read                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Commit format, delivery, push rules, issue labels | [contributing.md](./contributing.md)                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Harness runtime ports                             | [harness-runtime-ports.md](./harness-runtime-ports.md) — which runtime ports (fork, isolated spawn) each agent harness supports; read before an execution-placement decision                                                                                                                                                                                                                                                                                                     |
| Service orchestration in this workspace           | Both providers are bound via `[capabilities]`. tmux manifest: [config.toml](../../.winter/config/winter-service-tmux/config.toml) + [layout-hook.sh](../../.winter/config/winter-service-tmux/layout-hook.sh); docker manifest: [config.toml](../../.winter/config/winter-service-docker/config.toml) + compose files. Conventions in `winter-service-tmux:/index.md` and `winter-service-docker:/index.md`; [project-setup.md](./project-setup.md) has the both-provider setup. |

## Per-repo conventions

Each project repo ships its own `CONTRIBUTING.md` and `context/` directory. Read the one for the repo you are touching
before making changes. The workspace-level `contributing.md` above only covers the cross-repo flow (rebasing onto
`origin/master`, conventional commits with scope, `Closes #N` footers).
