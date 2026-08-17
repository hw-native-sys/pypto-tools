# 快速上手

本节以一次典型的 PyPTO 3.0 分析过程为例。

## 1. 打开输出目录

使用 VS Code 打开包含 PyPTO 输出文件的工作区。典型结构如下：

```text
build_out/
└── <case>/
    ├── dfx_outputs/
    │   ├── chip_swimlane_records.json
    │   ├── deps.json
    │   ├── name_map_<timestamp>.json
    │   ├── CPM_static.json             # 可选
    │   └── CPM_observed.json           # 可选
    └── passes_dump_<timestamp>/
        ├── 00_frontend.py
        ├── 01_<PassName>.py
        └── 02_<PassName>.py
```

关联文件尽量保持在同一目录。插件会自动使用 `deps.json` 和 `name_map*.json` 为泳道任务补充函数名及依赖信息。

## 2. 查看运行时泳道

在资源管理器中右键 `chip_swimlane_records.json`，选择 **PyPTO Toolkit：打开文件**。插件会根据文件实际包含的记录生成 Worker、Scheduler 和 Orchestrator 等视图。

![打开 Chip Swimlane](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/1_chip_swim_open.gif)

点击一个任务可以查看执行时间、Ring/Task ID、函数信息以及前后驱关系。若缺少关联文件，泳道仍可打开，但部分名称或依赖连线会降级显示。

## 3. 查看任务依赖

右键同目录中名称精确为 `deps.json` 的文件并选择插件打开入口。可以搜索任务、切换布局，并在 Full、Reduced、Omitted 等边模式之间切换，以检查冗余依赖。

## 4. 查看 IR Pass 变化

在资源管理器中右键名称以 `passes_dump` 开头的目录，选择 IR Pass Trace 入口。左侧选择 Pass 后，右侧会显示该 Pass 前后快照的差异。

![打开 IR Pass Trace](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/pass_IR_trace.gif)

## 下一步

- [了解各输入文件之间的关系](data-files.md)
- [分析 Chip Swimlane](../runtime/chip-swimlane.md)
- [分析任务依赖图](../runtime/dependency-graph.md)
- [使用 IR Pass Trace](../compiler/ir-pass-trace.md)
