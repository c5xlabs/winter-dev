# winter-dev

The C5X Labs meta workspace: a winter workspace for developing winter itself.

Project repositories are developed in coordinated feature environments; standalone extensions supply the workspace's
installed tooling and governance. It's the dogfooding setup — winter, used to build winter.

> **Note:** while this is technically eating our own dogfood, please do not eat yellow snow.

## Project repos

Declared in `.winter/config.toml` and managed by `winter ws init`:

- **[winter](https://github.com/c5xlabs/winter)** — the platform launcher repo
- **[winter-planning](https://github.com/c5xlabs/winter-planning)** — suite product planning
- **[winter-strategy](https://github.com/c5xlabs/winter-strategy)** — private platform strategy
- **[winter-context](https://github.com/c5xlabs/winter-context)** — shared engineering governance
- **[winter-context-planning](https://github.com/c5xlabs/winter-context-planning)** — product and engineering planning governance
- **[winter-test-service](https://github.com/paul-gross/winter-test-service)** — full-stack service fixture
- **[blizzard-product](https://github.com/paul-gross/blizzard-product)** — Winter Fleet product planning
- **[winter-workspace](https://github.com/paul-gross/winter)** — the legacy workspace framework (Python CLI, workspace
  skills, docs)
- **[legacy-winter-context](https://github.com/paul-gross/winter-context)** — legacy Python conventions, exemplars,
  README guide; retained for development, not installed as governance

The names above are local checkout names. GitHub repositories and the installed `winter` command keep their own
identities. Project source checkouts live in `projects/<name>/`; development happens in `<env>/<name>/` worktrees.

## Installed extensions

Standalone repos in `.winter/config.toml` are installed once under `.winter/ext/`, not worktreed per feature:

- **[winter-canon](https://github.com/paul-gross/winter-canon)** — universal harness conventions
- **[winter-context](https://github.com/c5xlabs/winter-context)** — shared engineering governance
- **[winter-github](https://github.com/paul-gross/winter-github)** — GitHub issue tooling
- **[winter-service-tmux](https://github.com/paul-gross/winter-service-tmux)** — tmux service orchestration
- **[winter-service-docker](https://github.com/paul-gross/winter-service-docker)** — Docker service orchestration
- **[winter-service-process-compose](https://github.com/paul-gross/winter-service-process-compose)** — process-compose service orchestration

`winter-context` is both a development project and an installed standalone; its installed copy is updated independently
of feature worktrees. `winter-context-planning` supplies governance directly from its project checkout.

Personal extensions belong in the gitignored `.winter/config.local.toml`. For example, to use
[winter-workflow](https://github.com/paul-gross/winter-workflow) locally, add:

```toml
[[standalone_repository]]
name = "winter-workflow"
url = "git@github.com:paul-gross/winter-workflow.git"
path = ".winter/ext/workflow"
```

Run `winter ws init` after changing the repository declarations. Workflow-specific agent model overrides also belong in
the local config. The shared workspace does not require winter-workflow.

## Getting started

1. Clone this repo.
2. Install the winter CLI (`./tools/winter-cli/install.sh`) and run `winter ws init` to clone projects and standalone extensions.
3. Run `winter ws init alpha`, then `winter provision alpha` before running services or exercising the environment.
4. Work in `alpha/`. Each project repo is a worktree on branch `alpha`.

See `CLAUDE.md` for workspace conventions and `context/project/contributing.md` for delivery rules.

## Running the legacy workspace CLI from a feature worktree

The installed `winter` script normally runs the CLI checked into `tools/winter-cli/` at the workspace root (i.e.
master). To exercise an in-flight CLI change living in a feature worktree without reinstalling, prefix the command with
`--winter=PATH`:

```sh
# from anywhere inside the workspace:
winter --winter=./alpha/winter-workspace dashboard
winter --winter=./alpha/winter-workspace ws status alpha
```

`PATH` is the feature worktree's checkout of `winter-workspace` (the directory containing `tools/winter-cli/`). The
launcher swaps the source tree but keeps the workspace root, config, and standalone extensions exactly the same — only
the Python code that runs is different.

`--winter=` must be the **first** argument; everything after it is forwarded to the alpha CLI verbatim.

## What this is *not*

This repo isn't the winter framework. It is a fork of [`winter`](https://github.com/paul-gross/winter) (upstream remote:
`winter`) plus one workspace-configuration commit, and it uses winter to develop winter — every change to the framework
or its extensions flows through a feature worktree here. Pull framework updates with the `update` skill (rebases onto
`winter/master`).

It replaces `paul-gross/winter-workspace`, which is left as is. The two can sit side by side: this one uses service
prefix `wdv` and ports from 6000.

## License

Source-available under the Functional Source License 1.1 (FSL-1.1-ALv2). Each release converts to Apache License 2.0 two
years after it ships. See LICENSE.
