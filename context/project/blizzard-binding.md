# This workspace's Blizzard binding

Read before any fleet call. General product usage belongs to [blizzard-usage](../../.winter/ext/usage/index.md); this
page supplies the local target and authority.

## Target and access

| Fact                 | Binding                                                                                                      |
| -------------------- | ------------------------------------------------------------------------------------------------------------ |
| Hub / board          | `https://blizzard.grosscode.net` — production, the real fleet, not a development or test hub                 |
| Tenant               | `paul-gross`, ID `ten_01M4N8ZW1AXGMFD9WCAFJ1FJZB`                                                            |
| Project              | `winter` — the project slug for this workspace; do not select `blizzard` for Winter work                     |
| Human authentication | GitHub OAuth through `blizzard hub login`; reads also require a session                                      |
| CLI                  | An operator-provided `blizzard` on `PATH`, compatible with the deployed fleet; if unavailable, ask for setup |

Use these shell bindings with the examples in `blizzard-usage`:

```bash
export HUB_URL=https://blizzard.grosscode.net
export TENANT=ten_01M4N8ZW1AXGMFD9WCAFJ1FJZB
export PROJECT=winter
```

Pass the tenant and hub explicitly on tenant-scoped calls, and the project on project-scoped calls. Confirm the project
before a mutation:

```bash
blizzard hub project get "$PROJECT" --tenant "$TENANT" --hub-url "$HUB_URL"
```

The project slug is the recorded selector here; take its stable `project_id` from that read when an interface requires
an ID. Do not invent one or reuse the Blizzard project's identity. Credentials and session tokens stay outside this
repository. Follow the [usage connection guidance](../../.winter/ext/usage/context/connection.md) for sign-in and access
problems; a source or feature checkout is not an approved installed CLI.

## Authority and machine ownership

An interactive assistant acts for the operator within the user's request. It is not a fleet worker without an assigned
lease and injected worker interface. Read access does not authorize ingestion, promotion, answers, merges, or other
mutations beyond the task.

The shared fleet's runner host is `xps`. This workspace does not own its deployment or service runbook. Host operations
and deployment changes belong to the operator in the Blizzard deployment workspace on `xps`, not to a Winter feature
environment. Access to the hosted hub from this machine does not grant runner-host access.

Adding a repository to Winter's local configuration does not register it with the fleet. Do not create hub repository,
source, or project links as a side effect of workspace setup. Follow this workspace's
[contributing conventions](./contributing.md) when preparing or delivering Winter work; a landing does not authorize a
fleet deployment.

## Operator workflows

Use the [shared operator workflows](blizzard-usage:/context/workflows/index.md) for marshalling, tending the garden, or
reviewing proposals. Gardening uses Winter's own scope ground and criteria registry, not Blizzard-development criteria;
resolve those from the selected routine and target's context before recommending runs.

For a request to marshal Winter's backlog, use the
[shared marshalling workflow](blizzard-usage:/context/workflows/marshalling.md) with this binding. Establish the
intended backlog sources, any manual-only or release-gated work, priority, and approval checkpoints with the operator
when the request leaves them unspecified. Do not import Blizzard-development labels or selection policy into Winter.
