# pypto-tools 文档

本仓库为两个相互独立的工具提供文档和 Release 附件。

| 工具 | 用途 | 文档 |
|---|---|---|
| **PyPTO3 Toolkit** | 用于 PyPTO 3.0 运行时和编译分析的 VS Code 插件 | [用户文档](toolkit/index.md) |
| **task-submit** | 共享 Ascend 主机上的 NPU 任务队列 | [文档](task-submit/index.md) |

两个工具互不依赖。

## 发布

每个工具使用独立的 tag 前缀，请在 [Release 列表](https://github.com/hw-native-sys/pypto-tools/releases) 中按前缀筛选。

| 工具 | Tag 前缀 | 附件 |
|---|---|---|
| PyPTO3 Toolkit | `v*` | VSIX |
| task-submit | `task-submit-v*` | `task-submit-<version>.tar.gz` 和 `.sha256` |

## 仓库范围

本仓库只存放文档和 Release 附件。插件源码不在本仓库维护；`task-submit` 的源码仓库是 [pypto-tools/npu-taskqueue](https://github.com/pypto-tools/npu-taskqueue)。
