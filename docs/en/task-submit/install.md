# Install and Upgrade

## Before you begin

- Install only on a shared Ascend host where the queue service is administered.
  The installer configures a root daemon and system-wide commands.
- An existing tools root (`/home/pypto-tools/pto-task` by default) is preserved:
  `config/` and `state/` are not overwritten by an upgrade.
- Python 3.9 or later and systemd must be available.

## Download and verify

1. Open the [task-submit release list](https://github.com/hw-native-sys/pypto-tools/releases?q=task-submit-v&expanded=true)
   and choose a `task-submit-v*` release.
2. Download `task-submit-<version>.tar.gz` and `task-submit-<version>.tar.gz.sha256`.
3. Verify the archive and extract it:

```bash
sha256sum -c task-submit-<version>.tar.gz.sha256
tar xzf task-submit-<version>.tar.gz
cd task-submit-<version>
```

## Install

```bash
sudo bash deploy.sh
```

`deploy.sh` installs the application files, creates a missing configuration,
enables `pto-task.service`, starts the daemon, and reports its status. Re-running
it from a new release is the supported upgrade path.

Use `setup.sh` instead when you want to install or update files without starting
or restarting the daemon:

```bash
sudo bash setup.sh
```

Common options:

| Option | Purpose |
|---|---|
| `--tools-root DIR` | Install below a different root (default `/home/pypto-tools`) |
| `--max-concurrent N` | Maximum simultaneously running jobs |
| `--available-devices LIST` | Auto-allocation pool, for example `0,1,2,3` |
| `--task-execution-mode MODE` | `HwHiAiUser` (default) or `root` |
| `--ptoas-base DIR` | Root containing installed PTOAS versions |
| `--non-interactive` | Never prompt; use options or detected defaults |

An interactive first install prompts for the card count and maximum concurrency.
Explicit options always take precedence.

## Verify the installation

```bash
systemctl status pto-task.service --no-pager
task-submit --list
task-submit --devices status
```

`--devices status` reports the auto-allocation pool and any project policy that
narrows it for the current directory.

## Upgrade

Download the new release, verify it, and run `sudo bash deploy.sh` from the new
directory. Configuration and queue state are preserved. If a task is running,
the files are updated but the daemon is not restarted; rerun the same command
after the queue drains.

A host can also receive updates from the source repository's rollout control.
That channel is configured on the host and does not require this page.

## Roll back

Download the older `task-submit-v*` release, verify it, and run `deploy.sh` from
that directory. Queue state is not rolled back.

## Uninstall

```bash
sudo systemctl disable --now pto-task.service
sudo rm -f /etc/systemd/system/pto-task*.service /etc/systemd/system/pto-task*.timer
sudo rm -f /etc/systemd/system/taskqueue.service
sudo systemctl daemon-reload
sudo rm -f /usr/local/bin/task-submit /usr/local/bin/pto-task /usr/local/bin/pypto-setup
sudo rm -rf /home/pypto-tools/pto-task
```

Adjust the tools root if it was changed at install time. Removing the tools root
also removes local configuration and queue history; back it up first if needed.

## Troubleshooting

- **The service is inactive.** Run `systemctl status pto-task.service --no-pager`
  and check the daemon log under `<tools-root>/pto-task/logs`.
- **No devices are available.** Check `task-submit --devices status` and the
  `--available-devices` setting in `<tools-root>/pto-task/config/taskqueue.conf`.
- **The job cannot see a device.** Pass the allocated device through
  `$TASK_DEVICE` inside the job; see [Basic usage](usage.md).

For configuration and scheduler details, see the
[source repository README](https://github.com/pypto-tools/npu-taskqueue#readme).
