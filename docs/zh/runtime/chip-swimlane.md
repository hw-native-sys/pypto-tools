# Chip Swimlane

Chip Swimlane 用于查看 PyPTO 3.0 任务从调度到 AICore 执行的时序。插件可直接读取 `chip_swimlane_records.json`，不需要先转换为 Chrome Trace。

## 打开泳道

在 VS Code 资源管理器中右键 `chip_swimlane_records.json`，选择 **PyPTO Toolkit：打开文件**。

![打开 Chip Swimlane](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/1_chip_swim_open.gif)

插件根据输入文件实际包含的数据生成视图：

| 视图 | 说明 |
|---|---|
| Worker View | 展示任务在 AIC/AIV Core 上的开始、结束和执行时长 |
| Scheduler View | 展示 AICPU 从 dispatch 到 finish 的调度区间 |
| Scheduler Phase | 展示 dispatch、early_dispatch、wire、release、resolve、drain 等阶段 |
| Orchestrator Phase | 展示 Orchestrator 提交阶段及其与 Scheduler 的关联 |

低记录级别不会凭空推导高级调度信息。例如，只包含 Worker 任务的文件不会生成完整 Scheduler Phase。

## 查看任务详情与依赖

点击任务后，下方面板会显示任务时间、Ring/Task ID、函数信息、Setup 耗时和 fan-out 等信息。同目录存在 `deps.json` 时，还会显示任务依赖连线。

![查看任务详情](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/2_chip_swim_click_task.gif)

任务连线层级可配置：

- `0`：不显示依赖扩展；
- 正整数：显示指定层数的前后驱；
- 具体可选范围以当前插件界面为准。

同名 SPMD 任务被选中时会一起高亮。依赖路径中的同名 SPMD 任务会进行显示收敛，避免大量连线遮挡视图。

## 搜索任务

在搜索框中输入函数名或 Task ID，可以模糊匹配并定位任务。

![搜索任务](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/3_chip_swim_search.gif)

任务文字可选择显示函数名、`Ring & Task ID`，或同时显示两者。

## 时间参考线

可以在时间轴上添加参考线，也可以右键任务，在任务开始、结束以及同名 SPMD 任务的整体边界处画线。添加的观测线会显示对应的时间戳。再次单击观测线，可以关闭操作面板。

![添加时间参考线](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/5_chip_swim_set_line.gif)

![标记 SPMD 任务边界](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/6_chip_swim_set_spmd_line.gif)


## Setup 分析

对于包含 receive-to-start 数据的记录，插件将 receive → start 识别为本地 Setup：

- 在 Worker 任务块中独立绘制 Setup 区域；
- 可配置总耗时是否包含 Setup；
- 可单独关闭 Setup 绘制；
- 任务详情展示 Setup 耗时；
- 性能面板统计 Setup 最长任务及 Setup 占比最高任务；
- 函数级 Kernel 耗时统计中会扣除 Setup。

![配置 Setup 显示](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/9_chip_swim_show_setup.gif)

## 性能分析面板

点击右上角的 **性能统计** 打开性能分析面板。面板提供总体统计、按 Kernel 统计和多种调优分析；从统计项选择任务或任务路径时，泳道会定位并高亮对应记录。

### 概览与按 Kernel 统计

概览罗列泳道图中的关键信息。按 Kernel 统计以 `func_id` 分组，展示 Kernel 执行次数，以及单次执行的最大、最小和平均耗时。单次耗时按 `start` 到 `end` 计算，不包含 Setup 时长。点击表头旁的图标，可以按对应列排序。

![概览与按 Kernel 统计](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/chip_swim_performance_statistics.gif)

### 连续单依赖分析

如果一个任务只有一个入度，且它的前驱任务也只有这一个出度（即当前任务），可以考虑合并两个任务以减少调度开销。**连续单依赖分析** 会在表格中列出满足条件的任务链。点击表格中的任务路径，会在 Worker View 中高亮整条链，并弱化其他任务的着色。

![连续单依赖分析](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/chip_swim_continuous_single_dep.gif)

### 关键路径分析

`simpler` 仓库中的关键路径分析脚本会在 `dfx_outputs` 目录下生成 `CPM_static*.json` 或 `CPM_observed*.json`。选择相应结果后，Worker View 会高亮关键路径上的任务，并弱化其他任务的着色。

![关键路径分析](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/chip_swim_critical_path.gif)

> **重要说明**
>
> 插件只读取并高亮已有 CPM 结果，不会在 IDE 内计算或生成关键路径报告。

#### 间隙与 Blocker 分析

**间隙与 Blocker 分析** 会解析观测关键路径，梳理路径中每个任务的前置间隙，并判断间隙来自同一 Core 上的其他任务阻塞，还是来自当前任务等待其前驱任务完成。可以指定间隙阈值，超过阈值的前置间隙会以红色显示。

![间隙与 Blocker 分析](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/chip_swim_critical_analysis.gif)

在分析表格中右键任务记录行并选择 **绘制前置间隙**，可以标记间隙的开始和结束时间。参考线自带标签，便于在 Worker View 中定位间隙前后的任务。

![绘制前置间隙](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/chip_swim_critical_draw_gap.gif)

### Early Dispatch 分析

该分析统计符合 Early Dispatch 配置条件的目标任务，并检查它们是否在所有前驱任务中最后一个 `FIN` 事件之前完成 dispatch。右键任务记录行并选择 **绘制最大提前**，可以标记最大提前区间的开始和结束时间。参考线自带标签，便于在 Scheduler View 中定位该区间。

![Early Dispatch 分析](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/chip_swim_early_dispatch_gap.gif)


## 其他操作

- 鼠标悬浮泳道左侧区域，可将泳道置顶；
- 可将当前泳道图导出为 PNG；
- 顶部入口可跳转到当前目录、父目录或 `passes_dump*` 下关联的 IR Pass Trace。

| 操作 | 功能 |
|---|---|
| 鼠标滚轮 | 纵向移动 |
| `Ctrl` + 鼠标左键拖动 | 横向移动 |
| `Ctrl` + 鼠标滚轮 | 缩放 |
| `Alt` + 鼠标左键 | 手动测距 |
| `w` / `s` | 放大/缩小，按界面配置生效 |
| `a` / `d` | 横向移动，按界面配置生效 |
