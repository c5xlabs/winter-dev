# Kickoff — rewrite the Winter Workspace CLI in Rust

You are the lead of a monumental build: a full rewrite of the Winter workspace executable in Rust. It is built into the
existing repository — cloned in this workspace as `winter-workspace` — on its `rust-cli` branch, and eventually replaces
what is there; it is not a new repository. The result is one binary, `winter-workspace`: the program the platform shim
will launch for `winter workspace …`. It works identically to the Python tool and never executes Python. You run the
build from this workspace, `winter-dev`, as the program lead of the **ice-age** build process. The run will span many
sessions; you are the first.

> **For the owner — how to start this.** From the workspace root, launch the lead on Fable with auto-compaction at 300k
> tokens and Remote Control on:
>
> ```bash
> cd /home/pgross/projects/winter-dev
> claude --model fable --autocompact 300k --dangerously-skip-permissions \
>   --remote-control rust-winter-workspace \
>   "Read context/project/kickoff-rust-winter-workspace.md and follow it."
> ```
>
> Every later session uses the same flags with a different prompt:
>
> ```bash
> claude --model fable --autocompact 300k --dangerously-skip-permissions \
>   --remote-control rust-winter-workspace \
>   "Resume the ice-age run at .winter/workflows/2026-11-14-rust-workspace/"
> ```

## How to run it

Execute the ice-age process exactly as written: `winter-workflow:/methodology/build/ice-age/process.md`, also available
as the `ice-age` skill (prefixed `wf-` by default). If that file does not resolve, the `winter-workflow` extension is
not installed here: stop and say so rather than improvising the process.

The program directory already exists, holding a draft charter and `open-decisions.md`, the owner's answers to the
backlog's open decisions:

```text
<workspace>/.winter/workflows/2026-11-14-rust-workspace/
```

Start at step 1. Check the draft charter against the workspace as you find it, correct what has gone stale, present it,
and wait for approval. It has no `Approved:` line, so nothing else may start before that.

## Where the work happens

| What             | Where                                                                                   |
| ---------------- | --------------------------------------------------------------------------------------- |
| Rust code        | `winter-workspace` repository, on its `rust-cli` branch                                 |
| Backlog          | `winter-planning` repository, `projects/workspace/`                                     |
| Planning rules   | `winter-context-planning` extension — the framework refinement follows                  |
| Reference        | The Python tool: `projects/winter-workspace/tools/winter-cli/`, `master` at `b57b020`   |
| Rust conventions | `winter` repository (the platform) — its `context/`, toolchain pin, and `test-harness`  |
| Integration      | Environment `alpha` — see below                                                         |
| Building         | Environments `beta`, `gamma`, `delta`, `epsilon`                                        |

The owner offers five environments for this run: `alpha`, `beta`, `gamma`, `delta`, and `epsilon`. On 2026-11-14 all
five existed, were provisioned, and were clean in both target repositories, level with `winter-workspace` `b57b020` and
`winter-planning` `ca3599a`. Confirm that still holds before using them, and create no others without asking.

- **`alpha` is the integration environment.** Its `winter-workspace` worktree has `rust-cli` itself checked out, so
  `rust-cli` is the integration line for code; its `winter-planning` worktree is on branch `alpha`, which is the
  integration line for the backlog. No slice is built in `alpha`; refinement of the backlog happens there.
- **`beta`–`epsilon` are the pool**: four slices in flight at most, and never more than eight roles running at once.
  Work one segment at a time; refining a segment takes three or four roles, not a fan-out. Their worktrees are on
  branches named for the environment.

Every edit happens in an environment's worktree — `<workspace>/<env>/<repo>/` — never in a source checkout under
`projects/`. Run `winter provision <env>` again for any environment whose readiness is in doubt.

Read-only for this run: the `winter` platform repository, and everything under `projects/`.

## What not to touch

- **Other environments.** `bz1`–`bz8` belong to other work.
- **The installed `winter` command.** It is the Python tool, it operates this workspace, and it is the oracle. The Rust
  binary is `winter-workspace`; it never replaces, shadows, or is installed over `winter`.
- **The Python reference.** The foundation's first slice wipes `tools/`, `context/`, and `skills/` from `rust-cli`, so
  the branch holds no Python and none of its documentation. Read and run the Python tool only from the source checkout
  `projects/winter-workspace` (`master`, `b57b020`), and read its manual under this workspace's `context/winter/`.
  Never modify either, and never wipe anything on a branch other than `rust-cli` and the slice branches cut from it.
- **Real workspaces.** Scenarios are recorded from the Python tool, and replayed by the Rust tests, only against fixture
  workspaces built in temporary directories — never against `winter-dev` or any workspace on this machine.
- **Remotes.** No push, no forge issues, no CI. Everything is local commits.
- **Out of scope:** the launcher shim and every `projects/platform` epic, releases and packaging, and Fleet.

## Models

Use Fable for everything: yourself and every role you spawn, with no exceptions. The roles' own definitions default to
other models, so:

1. Every `winter-workflow` agent is already retargeted to the `fable` tier in `[agent_model_overrides]` in the
   gitignored `.winter/config.local.toml`. Confirm it with `winter agents` before the first spawn; the table and its
   rules are at `workspace:/context/winter/cli/configuration/agents.md`.
2. Pass the Fable model explicitly on every spawn as well, and tell any role allowed to spawn its own helpers to do the
   same.
3. Spawn one `ice-carver` with a one-line probe and confirm from its transcript that it ran on Fable before dispatching
   real work. If any role is found running on another model, stop dispatching and say so.

## Working with the owner

The owner follows this run by remote control from a phone and answers promptly. Ask rather than guess, as the process's
Asking section describes — and make every question answerable in a tap: one line on what it blocks, the options, and
your recommendation first.

Keep your own context small. Your session auto-compacts at 300k tokens, so a compaction can arrive without a request:
keep `plan.md` and the ledger current enough that losing your context costs nothing, and after any compaction re-read
the process and run its Resuming section before the next dispatch. The budget for any role you spawn is 400k.

## Known state on 2026-11-14

- There is no Rust code yet: `rust-cli` equals `master`.
- The backlog is 29 epics at `outline`, about 140 requirements, no specs, and no slices. Refine it one segment at a
  time, just before building that segment.
- The platform pins Rust 1.99.1; this machine had 1.96.0. Toolchain and crate downloads need the network.
- The three service providers — tmux, docker, process-compose — are Python extensions outside this workspace's project
  repositories, installed under `.winter/ext/service-*`. The owner has decided they are ported into the binary as
  built-ins, with the external provider contract kept for further providers; how the two coexist is refined when that
  segment is reached.
- Provisioning for `winter-workspace` tolerates both layouts: it runs `uv sync` where `tools/winter-cli` exists and
  `cargo fetch` where a root `Cargo.toml` exists.
- The owner's end goal is that agents get the CLI's context from a `context` command of the binary, not from markdown
  files in the repository. That command is new; design it with the owner when its segment is reached.
- The owner wants full specs written for the Rust application in every segment's refinement, to read before the build.

## The next session

Every later session starts from the program directory, not from this file: "resume the ice-age run at
`.winter/workflows/2026-11-14-rust-workspace/`".
