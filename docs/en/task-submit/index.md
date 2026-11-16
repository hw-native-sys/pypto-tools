# task-submit

`task-submit` submits and runs NPU jobs on a shared Ascend host. A root daemon
keeps a queue, allocates and locks devices, and runs each job as the user who
submitted it. `pto-task` is an equivalent alias.

> **Scope**
>
> `task-submit` is distributed from this repository but is not part of the
> PyPTO3 Toolkit extension. Install it only on a shared Ascend host where the
> queue service is administered.

## What the package contains

| Component | Purpose |
|---|---|
| `task-submit`, `pto-task` | Client commands for submitting and inspecting jobs |
| `task-daemon.sh`, `pto-task.service` | Root queue daemon and its systemd unit |
| `deploy.sh`, `setup.sh` | Install, update, and activate the service |
| `skills/pto-task-operations/` | Operation guide for AI assistants and scripts |
| `tests/` | Verification run by the release process |

## Relationship to PyPTO3 Toolkit

- The two products are independent: installing one does not install the other.
- The extension reads runtime capture files such as `chip_swimlane_records.json`.
  `task-submit` is one way to run the workloads that produce those files.
- `task-submit` has its own release line (`task-submit-v*`) and its own source
  repository, [pypto-tools/npu-taskqueue](https://github.com/pypto-tools/npu-taskqueue).

## Host requirements

| Requirement | Detail |
|---|---|
| Ascend NPU with CANN | A host that can run the workloads you submit |
| Linux with systemd | The daemon is installed as `pto-task.service` |
| Python 3.9 or later | Used by the daemon and the client |
| Root access | Required to install, upgrade, and manage the service |
| Shared-host policy | One queue daemon per host; do not start a second instance |

## Releases and versions

`task-submit` releases use the `task-submit-v*` tag prefix, separately from the
extension's `v*` releases. Use the
[filtered release list](https://github.com/hw-native-sys/pypto-tools/releases?q=task-submit-v&expanded=true)
to find the latest package. If no `task-submit-v*` entry is listed, no package
has been published yet.

Each release names the source commit and the SHA-256 of its tarball.

## Next steps

- [Install and upgrade](install.md)
- [Basic usage](usage.md)

For the complete command reference, scheduler policies, and configuration, see
the [source repository README](https://github.com/pypto-tools/npu-taskqueue#readme).
