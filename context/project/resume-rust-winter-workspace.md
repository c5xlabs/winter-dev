# Resume — the Rust rewrite of the Winter Workspace CLI

Written 2026-11-16, when the run was paused by the owner at the end of the Fable usage budget. Read with
[kickoff-rust-winter-workspace.md](./kickoff-rust-winter-workspace.md), which still states the rules of the run; this
file states where the run stands and what the next session must know. The run's own records are in the program
directory, which is gitignored and exists only on this machine:

```text
<workspace>/.winter/workflows/2026-11-14-rust-workspace/
```

## Before the next session starts

1. **The model policy must be changed first.** The kickoff and `charter.md` bind every role to Fable and tell the lead
   to stop dispatching when a role runs on another model. To continue on another model: amend the Models section of the
   kickoff and "The model policy" of the charter (an amendment at its end, as the existing one), retarget
   `[agent_model_overrides]` in `.winter/config.local.toml`, and confirm with `winter agents`. Every role prompt in
   this run passed `model: fable`; the next lead passes the new model instead.
2. Start as the kickoff says: "Resume the ice-age run at `.winter/workflows/2026-11-14-rust-workspace/`". The lead reads
   the charter, `plan.md`, the ledger, `questions.md`, the tail of `journal.md`, then follows the Resuming section of
   `winter-workflow:/methodology/build/ice-age/process.md`, including the slice gate on `alpha` before any dispatch.

## Where the run stands

| What                 | State                                                                                          |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| Segments done        | 1 Foundation, 2 `component-runtime` — checkpoints in the program directory's `checkpoints/`    |
| Segment in progress  | 3 `parallel-workspaces/workspace-configuration`: nine of ten slices integrated                 |
| Code, local          | `alpha/winter-workspace`, branch `rust-cli` at `4fc08ee`; full gate green, 93 scenarios        |
| Code, pushed         | `origin/rust-cli` at `4da904c` — everything of segment 3 is local only                         |
| Backlog, local       | `alpha/winter-planning`, branch `alpha` at `946c68e`                                           |
| Backlog, pushed      | `origin/master` at `1f682bb` — the segment-3 specs and slices are local only                   |
| Pool                 | `beta`, `gamma`, `delta`, `epsilon`: all clean; code at or behind `4fc08ee`, backlog `946c68e` |
| Real commands so far | `ws index` and `repo remove`, both loading and validating the whole configuration              |
| Segments remaining   | about 24, from `repository-catalog` onward through `epics.md`                                  |

The unpushed work exists only in this machine's worktrees. Push it or back it up before anything that could lose them.

## What is left of segment 3

1. **Slice 8, `configuration-proof`** (`slices/08-configuration-proof.md` under the epic's directory in the backlog):
   five plain recorded scenarios, one unit test, one component test; tests only. Its first builder was stopped a few
   minutes in, with nothing written; `beta` is clean. Build, verify and review, integrate.
2. **The checkpoint**: full gate on `alpha`, acceptance against the epic's requirements, the architecture, context and
   documentation reviews over `4da904c..<tip>`, repair slices for blocking findings, the checkpoint file
   `checkpoints/03-workspace-configuration.md`, then the owner's word on continuing.

## Owed, and where it is written

- **Before the next push of `rust-cli`:** three recorded reference outputs hold a path under the owner's home directory
  (`scenarios/ws-index/{config-not-utf8,state-not-utf8,state-is-directory}/record/steps/01/stderr`). The recorder does
  not normalise the interpreter's library path; fix the recorder and re-record, or accept it. When `config-not-utf8` is
  re-recorded, make its fixture valid TOML apart from one bad byte.
- **Pushes happen only on the owner's word.** `rust-cli` goes to `origin/rust-cli`; the backlog is rebased onto
  `origin/master` and pushed there.
- **Four contradictions in the backlog** between the upstream launcher text (Workspace started through the platform,
  version cache, release pin) and the rewrite's text (one binary started directly). The owner settles them later; the
  build follows the rewrite's side. Listed in `reports/push-01/rebase-1.md`.
- **Backlog corrections owed**, slice by slice, are in `register.md` — small places where a spec lags the code.
- **Deferred questions** Q5, Q6, Q8, Q41, Q42 are in `questions.md`, each with the segment that needs its answer.
- **Slice PV/13 `program-capture`** waits in the ledger; it is built before the first segment that runs external
  programs.

## Decisions that shape everything after

All answers are in `questions.md` (Q1–Q49) and in the backlog's `decisions.md` files; the ones a new lead most needs:

- **Functional parity, not byte parity** (charter amendment). A difference in function is asked; wording is free.
- **Registered deviations**: every accepted functional difference has a row in
  `component-runtime/spec/command-surface.md` and a scenario in the deviation form. Mistyped configuration is refused
  where the reference tolerates it; a `state.toml` that cannot be read or parsed is a configuration error.
- **Configuration writes** are atomic replace-by-rename under a lock; the reference writes in place.
- **Tracing** uses the OpenTelemetry crates, is off unless `WINTER_OTEL_EXPORTER_OTLP_ENDPOINT` is set (the same
  variable the installed `winter` reads), and is bounded to 100 ms.
- **Usage errors** exit 2: a repeated flag, non-UTF-8 arguments, `--` before a command name.

## Safety rules learned in the run

These are in every role prompt and must stay there; each was added after a role slipped.

- The built binary runs only from `env -i` inside a fixture workspace under the role's scratch directory. `repo remove`
  rewrites the configuration of whatever workspace lies above its working directory. Never under
  `/home/pgross/projects`, never installed, never on `PATH`.
- The Python reference is started only through `cargo dev record … --reference <path>`; no role types `python`.
- One scratch directory per role at a literal path `/tmp/winter-run/<slice>-<role>-<n>`, removed by one literal
  `rm -rf` of that path alone in its own shell call. No other recursive removal.
- `CARGO_NET_OFFLINE=true` on every cargo call; no connections to fixed ports; signals only to a role's own children by
  the pid the start returned, never core-dumping ones; no `git stash`; files written with the edit tools (one literal
  `printf` is allowed for a fixture that needs non-UTF-8 bytes).
- No process references in code or agent-facing markdown; the commit message carries provenance.
- The lead reads every backlog author's diff line by line; whole-file rewrites corrupted unrelated lines twice.

## What the run cost, and what to change

- Three segments took roughly two days of wall time and the whole weekly Fable budget: about 8M role tokens for the
  foundation, 9M for segment 2, and segment 3 in the same range. At that rate the remaining segments do not fit a week
  each on the same budget.
- Where the tokens went: every slice is built, then verified and reviewed by two fresh roles, each reading the
  repository's context and the specs anew (70k–170k tokens per role); refinement of a segment took three gates.
- Cheaper without losing much, for the owner to choose: one combined verify-and-review role for test-only and
  scenario-only slices; larger slices where the gate allows; the adversarial review only for slices that change
  behaviour; a lighter model for verification and scenario recording, keeping the strongest for specs and review.
- What worked: recorded scenarios against the reference caught real differences in every segment; specs written before
  the code made the owner's questions small and answerable in a tap; holding a slice at its attempt cap and asking
  never cost more than one more round.
