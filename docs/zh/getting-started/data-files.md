# 准备分析文件

PyPTO Toolkit 不负责采集或生成 PyPTO 运行记录；它负责读取框架已经输出的文件并提供可视化。输入文件越完整，可展示的信息越丰富。

## 运行时文件

| 文件 | 能否独立打开 | 作用 |
|---|---:|---|
| `chip_swimlane_records.json` | 是 | 提供任务、Core、Scheduler、Orchestrator 和时间信息 |
| `serving-strace-swimlane.json` | 是 | 提供 Serving 任务时序，并按 `WorkerProcess` 汇总任务数量和耗时 |
| `deps.json` | 是 | 提供任务 DAG、Tensor 信息和泳道依赖连线 |
| `name_map*.json` | 有条件 | 提供 Func ID 到函数名的映射，并结合泳道生成函数性能表 |
| `CPM_static*.json` | 否 | 为泳道提供已经计算出的静态关键路径任务 |
| `CPM_observed*.json` | 否 | 为泳道提供已经计算出的观测关键路径任务 |

建议目录：

```text
dfx_outputs/
├── chip_swimlane_records.json
├── deps.json
├── name_map_<timestamp>.json
├── CPM_static.json             # 可选
└── CPM_observed.json           # 可选
```

`serving-strace-swimlane.json` 可以独立打开，不依赖上述 Chip Swimlane 关联文件。

打开泳道文件时，插件会尝试读取同目录中的 `deps.json` 和 `name_map*.json`：

```text
chip_swimlane_records.json
├── deps.json       → 任务依赖、kernel_ids、Func ID
└── name_map*.json  → Func ID 到函数名
```

缺少关联文件时的影响：

- 缺少 `deps.json`：无法完整恢复 Func ID，也不会显示数据依赖连线；
- 缺少 `name_map*.json`：部分任务只能显示 ID，不能显示函数名；
- 缺少 CPM 文件：不能选择相应关键路径高亮模式；
- 只有 `name_map*.json` 而没有泳道文件：函数性能表无法计算耗时。


## IR Pass 快照

IR Pass Trace 要求目录名以 `passes_dump` 开头，且目录中包含连续编号的 Python 快照：

```text
passes_dump_<timestamp>/
├── 00_frontend.py
├── 01_PassA.py
├── 01_PassA.log       # 可选，展示该 Pass 的 warning
├── 02_PassB.py
└── 03_PassC.py
```

约束如下：

- 必须存在 `00_frontend.py`；
- 后续文件名应符合 `NN_<PassName>.py`；
- Pass 序号从 1 开始连续，不能缺号或重复；
- 同名 `.log` 文件可选。

同一个 pass dump 也可以使用 `memory_map` 进行[内存复用分析](../compiler/memory-reuse.md)，查看每个 MemRef 的地址范围和生命周期。
