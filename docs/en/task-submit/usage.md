# Basic Usage

`task-submit` and `pto-task` are the same program. This page covers the commands
needed to submit and inspect a job. For the complete option and scheduler
reference, see the
[source repository README](https://github.com/pypto-tools/npu-taskqueue#readme).

## Submit a job

```bash
task-submit --device auto --run "python train.py"
```

- `--device auto` allocates free cards from the host pool.
- `--device-num N` requests several cards; the request must fit in one
  scheduling pool.
- Inside the job, `$TASK_DEVICE` holds the allocated device list. Expand it in
  the command when the program needs the device ID:

```bash
task-submit --device auto --run "python train.py --device \$TASK_DEVICE"
```

The daemon appends the allocated device to a simple command automatically. It
does not do so for compound commands or when the command already specifies a
device, so pass `$TASK_DEVICE` explicitly in those cases.

- `--max-time` limits execution after the job starts (default 300 seconds).
- `--timeout` limits only the pending queue wait (default 600 seconds).
- For queued work that does not use a card, submit with `--no-device`.

## Prepare outside the allocation

A submitted job holds its devices for the lifetime of the process. Build the
project, install dependencies, and generate inputs before submitting, then run
only the on-device step inside the job. If preparation must go through the same
queue, submit it separately with `--no-device`.

## Inspect jobs

| Command | Purpose |
|---|---|
| `task-submit --list` | List pending, running, and finished jobs |
| `task-submit --status <id>` | Show one job's state |
| `task-submit --log <id>` | Print a job's log |
| `task-submit --wait <id>` | Wait for completion and print the result |
| `task-submit --cancel <id>` | Cancel a pending job |
| `task-submit --kill <id>` | Kill a running job |
| `task-submit --devices status` | Show the auto-allocation pool and policy |

## Next steps

- [Install and upgrade](install.md)
- [task-submit overview](index.md)
