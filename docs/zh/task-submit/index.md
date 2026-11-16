# task-submit

`task-submit` 用于在共享 Ascend 主机上提交和运行 NPU 任务。root 守护进程维护队列、
分配并锁定设备，并以提交者的身份运行任务。`pto-task` 是等价别名。

> **范围**
>
> `task-submit` 由本仓库分发，但不属于 PyPTO3 Toolkit 插件。请只在由管理员维护的
> 共享 Ascend 主机上安装。

## 安装包内容

| 组件 | 用途 |
|---|---|
| `task-submit`、`pto-task` | 提交和查看任务的客户端命令 |
| `task-daemon.sh`、`pto-task.service` | root 队列守护进程及其 systemd unit |
| `deploy.sh`、`setup.sh` | 安装、更新并激活服务 |
| `skills/pto-task-operations/` | 供 AI 助手和脚本读取的操作规范 |
| `tests/` | 发布流程执行的校验 |

## 与 PyPTO3 Toolkit 的关系

- 两个产品相互独立：安装其中一个不会安装另一个。
- 插件读取运行时采集文件（例如 `chip_swimlane_records.json`）；`task-submit`
  是产生这些文件的运行方式之一。
- `task-submit` 有独立的发布线（`task-submit-v*`）和源码仓库
  [pypto-tools/npu-taskqueue](https://github.com/pypto-tools/npu-taskqueue)。

## 主机要求

| 要求 | 说明 |
|---|---|
| Ascend NPU 与 CANN | 能够运行所提交任务的主机 |
| 带 systemd 的 Linux | 守护进程以 `pto-task.service` 安装 |
| Python 3.9 或更高版本 | 守护进程和客户端使用 |
| root 权限 | 安装、升级和管理服务需要 |
| 共享主机约定 | 每台主机只运行一个队列守护进程 |

## 发布与版本

`task-submit` 使用 `task-submit-v*` tag 前缀发布，与插件的 `v*` 发布相互独立。
在[筛选后的 Release 列表](https://github.com/hw-native-sys/pypto-tools/releases?q=task-submit-v&expanded=true)
中查找最新安装包。如果列表中没有 `task-submit-v*` 条目，说明尚未发布安装包。

每个 Release 会注明源码提交和压缩包的 SHA-256。

## 后续步骤

- [安装与升级](install.md)
- [基本使用](usage.md)

完整命令、调度策略和配置说明见
[源码仓库 README](https://github.com/pypto-tools/npu-taskqueue#readme)。
