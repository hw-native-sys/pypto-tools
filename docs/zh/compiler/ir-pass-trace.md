# IR Pass Trace

IR Pass Trace 用于比较连续编译 Pass 前后的 Python IR 快照，帮助判断某个 Pass 修改了哪些函数，以及是否产生 warning。

## 输入要求

目录名称应以 `passes_dump` 开头，并满足以下约定：

```text
passes_dump_<timestamp>/
├── 00_frontend.py
├── 01_PassA.py
├── 01_PassA.log       # 可选
├── 02_PassB.py
└── 03_PassC.py
```

- 必须存在 `00_frontend.py`；
- 后续快照符合 `NN_<PassName>.py`；
- Pass 序号从 1 开始连续，不能缺号或重复；
- 与快照同名的 `.log` 文件可选，用于展示该 Pass 的 warning。

每个 Pass 项比较“上一份快照”和“当前 Pass 快照”。例如 `02_PassB.py` 会与 `01_PassA.py` 比较。

## 打开方式

在 VS Code 资源管理器中右键 `passes_dump*` 目录，选择 IR Pass Trace。也可以从 Chip Swimlane 顶部跳转到插件找到的关联目录。

![IR Pass Trace](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/pass_IR_trace.gif)

## 查看变化

左侧列表按序号显示 Pass，并统计新增和删除行数。界面支持：

- 标记没有代码变化的 No-op Pass；只看 Changed 或 No-op Pass；
- 标记并展示同名 `.log` 中的 warning；
- 默认定位到第一个发生变化的 Pass；
- 按顶层函数或类内方法过滤差异；
- 同时展示行级和字符级差异；
- 在 Side by side 和 Stacked 布局间切换；
- 双栏同步滚动；
- 折叠大段未变化代码，逐段展开或全部展开；
- 使用 `j`/`k` 或方向键上下切换 Pass；

## 使用建议

1. 先启用 **Changed** 过滤，快速找到真正修改 IR 的 Pass；
2. 按函数过滤，聚焦当前算子入口或可疑函数；

> **说明**
>
> IR Pass Trace 是只读预览工具，不会编辑或回写 `passes_dump` 文件。
