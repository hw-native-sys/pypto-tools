# 任务依赖图

任务依赖图读取 `deps.json` 中的 `tasks`、`edges` 和 `tensors`，展示 PyPTO 3.0 任务 DAG。插件会自动去除重复边，并尝试读取同目录最新的 `name_map*.json` 显示函数名。

## 打开依赖图

在 VS Code 资源管理器中右键名称精确为 `deps.json` 的文件，选择 **PyPTO Toolkit：打开文件**。

![打开任务依赖图](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/deps_open_file.gif)



## 节点信息

不同形状和颜色用于区分 AIC、AIV、MIX、ALLOC 和 SPMD 任务。节点及详情面板可以展示：

- 函数名和 Ring/Task ID；
- `block_num`、前驱数和后继数；
- Tensor 参数序号和输入输出类型；
- Tensor 名称、dtype、shape、stride 和 offset；
- 前驱、后继及SPMD同名任务归并数量。

节点块的样式和背景颜色由执行该任务所使用的 AICore 类型决定。SPMD 任务使用堆叠的显示效果，并在节点文本中显示重叠次数。节点两侧的端口（Port）标识入度和出度。

![任务依赖图节点](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/deps_node.png)

## 搜索、节点选择与依赖高亮

可以按函数名或 Task ID 搜索任务并定位。鼠标悬停或点击节点时，可高亮其前驱和后继。

点击任务节点后，右侧面板会显示任务详细信息，并高亮包含该节点的任务依赖链。通过依赖层级数字可以控制显示的依赖层数。

![任务依赖节点详情](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/deps_node_click.png)

依赖高亮层级：

| 值 | 含义 |
|---:|---|
| `0` | 关闭依赖高亮 |
| 正整数 | 展开指定层数的依赖 |
| `-1` | 展开全部传递依赖 |

图支持从左到右、从上到下两种布局，以及缩放、平移和 Fit to Screen。HUD 会显示节点数和当前可见边数。

## 依赖边模式

| 模式 | 含义 |
|---|---|
| Full | 显示全部依赖边 |
| Reduced | 隐藏传递冗余边 |
| Omitted | 只显示被判定为冗余的边 |
| Reduced DF | 按 Data Flow 规则隐藏冗余边 |
| Omitted DF | 只显示按 Data Flow 规则判定为冗余的边 |

建议先用 **Full** 确认原始依赖，再用 **Reduced** 或 **Reduced DF** 聚焦必要依赖；需要检查冗余边来源时切换到相应的 Omitted 模式。

浏览任务依赖图时，可以通过鼠标或键盘数字键切换不同的边渲染模式，并高亮需要关注的边。必要边和冗余边互为补集。普通冗余依赖分析默认 `alloc` 后续的依赖连线不可删除；生命周期分析会进一步判断这些依赖连线是否也可以删除。

![冗余依赖分析](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/deps_omitted_analysis.gif)

## Early Dispatch 标记

依赖图中的标记表达的是依赖描述或图结构信息：

| 标记 | 当前含义 |
|---|---|
| `🔥` | 任务为 ALLOC，或 `deps.json` 中带有 `early_dispatch` 标记 |
| `⭐` | 所有前驱均属于可提前处理集合，且至少有一个非 ALLOC 前驱 |

> **重要说明**
>
> 这些标记不等价于本次运行中真实提前调度成功。依赖图不能给出“某任务 N/M 个物理 Block 被提前调度”这样的运行时比例。Scheduler 泳道中的 `early_dispatch` phase 只能说明运行记录中出现了相应阶段。
