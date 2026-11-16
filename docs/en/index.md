# pypto-tools Documentation

Documentation and Release assets for two independent tools.

| Tool | Purpose | Documentation |
|---|---|---|
| **PyPTO3 Toolkit** | VS Code extension for PyPTO 3.0 runtime and compilation analysis | [User guide](toolkit/index.md) |
| **task-submit** | NPU job queue for a shared Ascend host | [Documentation](task-submit/index.md) |

Neither tool requires the other.

## Releases

Each tool has its own tag prefix; filter the [release list](https://github.com/hw-native-sys/pypto-tools/releases) by prefix.

| Tool | Tag prefix | Asset |
|---|---|---|
| PyPTO3 Toolkit | `v*` | VSIX |
| task-submit | `task-submit-v*` | `task-submit-<version>.tar.gz` and `.sha256` |

## Repository scope

This repository holds documentation and Release assets only. The extension source is maintained elsewhere; `task-submit` is developed in [pypto-tools/npu-taskqueue](https://github.com/pypto-tools/npu-taskqueue).
