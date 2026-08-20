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


## 性能面板

点击右上角的 **性能统计**，可以查看耗时和 Setup 相关统计。从统计项点击任务时，泳道会定位到对应记录。

![性能面板](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/7_chip_swim_perf.png)

## Setup 分析

对于包含 receive-to-start 数据的记录，插件将 receive → start 识别为本地 Setup：

- 在 Worker 任务块中独立绘制 Setup 区域；
- 可配置总耗时是否包含 Setup；
- 可单独关闭 Setup 绘制；
- 任务详情展示 Setup 耗时；
- 性能面板统计 Setup 最长任务及 Setup 占比最高任务；
- 函数级 Kernel 耗时统计中会扣除 Setup。

![配置 Setup 显示](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/9_chip_swim_show_setup.gif)

## 关键路径高亮

当同目录存在 `CPM_static*.json` 或 `CPM_observed*.json` 时，可在渲染配置中选择关键路径。非关键路径任务会被弱化，以便聚焦主要执行链。

![关键路径高亮](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/10_chip_swim_show_CPM.gif)

> **重要说明**
>
> 插件只读取并高亮已有 CPM 结果，不会在 IDE 内计算或生成关键路径报告。



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
