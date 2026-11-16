# 基本使用

`task-submit` 与 `pto-task` 是同一个程序。本页只介绍提交和查看任务所需的命令；
完整选项和调度说明见
[源码仓库 README](https://github.com/pypto-tools/npu-taskqueue#readme)。

## 提交任务

```bash
task-submit --device auto --run "python train.py"
```

- `--device auto` 从主机池中分配空闲卡。
- `--device-num N` 申请多张卡；请求必须落在同一个调度池内。
- 任务内部通过 `$TASK_DEVICE` 获取分配的设备列表。程序需要设备号时在命令中展开：

```bash
task-submit --device auto --run "python train.py --device \$TASK_DEVICE"
```

对于简单命令，守护进程会自动追加分配到的设备；复合命令或命令中已指定设备时不会
追加，此时需要显式传入 `$TASK_DEVICE`。

- `--max-time` 限制任务开始后的执行时间（默认 300 秒）。
- `--timeout` 只限制排队等待时间（默认 600 秒）。
- 需要排队但不使用卡的工作，使用 `--no-device` 提交。

## 在占卡外完成准备

提交的任务会在整个进程生命周期内占用其设备。编译、安装依赖和生成输入应在提交前
完成，占卡任务中只保留真正上板的步骤。如果这些准备工作必须经过同一个队列，请用
`--no-device` 单独提交。

## 查看任务

| 命令 | 用途 |
|---|---|
| `task-submit --list` | 列出待运行、运行中和已结束的任务 |
| `task-submit --status <id>` | 查看单个任务的状态 |
| `task-submit --log <id>` | 输出任务日志 |
| `task-submit --wait <id>` | 等待任务结束并输出结果 |
| `task-submit --cancel <id>` | 取消待运行任务 |
| `task-submit --kill <id>` | 终止运行中的任务 |
| `task-submit --devices status` | 查看自动分配池和策略 |

## 后续步骤

- [安装与升级](install.md)
- [task-submit 概览](index.md)
