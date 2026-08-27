# Serving Strace Swimlane

Serving Strace Swimlane 用于查看 `serving-strace-swimlane.json` 中记录的 Serving 任务时序，并按 `WorkerProcess` 汇总任务性能。

## 打开泳道

在 VS Code 资源管理器中右键 `serving-strace-swimlane.json`，选择 **PyPTO Toolkit：打开文件**。

![打开 Serving Strace Swimlane](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/serving-strace-swimlane_open.gif)

## 查看性能统计

性能面板按 `WorkerProcess` 统计以下指标：

- 任务数量；
- 平均耗时；
- 最大耗时；
- 最小耗时。

这些统计可以帮助比较不同 `WorkerProcess` 的任务规模和耗时分布。
