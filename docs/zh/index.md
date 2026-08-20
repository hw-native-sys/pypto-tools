# PyPTO Toolkit 用户文档

PyPTO Toolkit 是面向 PyPTO 算子开发者的 VS Code 插件。本说明聚焦插件为 **PyPTO 3.0** 提供的运行时和编译过程可视化能力，帮助用户查看任务执行时序、分析任务依赖，并定位编译 Pass 带来的 IR 变化。

## 主要能力

| 分析对象 | 输入 | 用途 |
|---|---|---|
| Chip Swimlane | `chip_swimlane_records.json` | 查看 AICore Worker、AICPU Scheduler 和 Orchestrator 的运行时序 |
| 任务依赖图 | `deps.json` | 查看任务、Tensor、前后驱关系和冗余依赖 |
| 函数性能表 | `name_map*.json` 和同目录泳道文件 | 按函数汇总执行次数及最大、最小、平均耗时 |
| IR Pass Trace | `passes_dump*` 目录 | 对比连续编译 Pass 前后的 Python IR 快照 |
| 内存复用分析 | 使用 `memory_map` 处理的 pass dump | 从地址和生命周期两个维度查看片上内存布局与复用 |

[安装插件](getting-started/install.md){ .md-button .md-button--primary }
[快速上手](getting-started/quick-start.md){ .md-button }

## 推荐分析顺序

1. 从最新 [GitHub Release](https://github.com/hw-native-sys/pypto-tools/releases/latest) 下载并安装 VSIX。
2. 将 PyPTO 3.0 运行生成的 `dfx_outputs` 和 `passes_dump*` 保留在 VS Code 工作区内。
3. 先打开 `chip_swimlane_records.json` 定位耗时和调度阶段。
4. 再打开同目录的 `deps.json` 检查任务依赖和冗余边。
5. 需要定位编译变化或使用 `memory_map` 分析内存复用时，打开对应的 `passes_dump*` 目录。

> **文档范围**
>
> 本文只介绍 PyPTO 3.0 已接通并可供用户使用的功能。

## 反馈

如遇到问题，请在 [hw-native-sys/pypto-tools Issues](https://github.com/hw-native-sys/pypto-tools/issues) 中提交问题，并尽量附上插件版本、输入文件类型、复现步骤和错误信息。使用插件前请阅读仓库 [中文 README 中的免责声明](https://github.com/hw-native-sys/pypto-tools/blob/main/README.zh.md#免责声明)。
