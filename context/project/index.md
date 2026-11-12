# Project context — building winter, with winter

This workspace develops the **winter** platform, its planning and governance, the service test fixture, and the legacy
workspace framework. The authoritative project and standalone lists are in
[`.winter/config.toml`](../../.winter/config.toml); the [README](../../README.md) describes their roles.

Shared engineering governance comes from the `c5xlabs/winter-context` standalone at `.winter/ext/context`; its project
checkout is for development. `legacy-winter-context` is not an active governance provider. Planning governance comes
from the `winter-context-planning` standalone at `.winter/ext/context-planning`; its project checkout is for development. Service providers and GitHub tooling are standalone extensions.

Personal extensions are declared in the gitignored `.winter/config.local.toml`. `winter-workflow` is optional, not a
shared workspace dependency.

## Project-level conventions

| Topic                                             | Where to read                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Commit format, delivery, push rules, issue labels | [contributing.md](./contributing.md)                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Blizzard fleet access                             | [blizzard-binding.md](./blizzard-binding.md) — read before any fleet call to select this workspace's target and establish authority.                                                                                                                                                                                                                                                                                                                                             |
| Harness runtime ports                             | [harness-runtime-ports.md](./harness-runtime-ports.md) — read before choosing an execution placement that depends on a harness capability.                                                                                                                                                                                                                                                                                                                                       |
| Service orchestration in this workspace           | Both providers are bound via `[capabilities]`. tmux manifest: [config.toml](../../.winter/config/winter-service-tmux/config.toml) + [layout-hook.sh](../../.winter/config/winter-service-tmux/layout-hook.sh); docker manifest: [config.toml](../../.winter/config/winter-service-docker/config.toml) + compose files. Conventions in `winter-service-tmux:/index.md` and `winter-service-docker:/index.md`; [project-setup.md](./project-setup.md) has the both-provider setup. |

## Per-repo conventions

Each project repo ships its own `CONTRIBUTING.md` and `context/` directory. Read the one for the repo you are touching
before making changes. The workspace-level `contributing.md` above only covers the cross-repo flow (rebasing onto
`origin/master`, conventional commits with scope, `Closes #N` footers).
