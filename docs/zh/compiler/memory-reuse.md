# 内存复用分析

`memory_map` 会将 pass dump 渲染为交互式的片上内存 HTML 地图。当需要理解编译结果如何使用和复用内存时，可以结合 [IR Pass Trace](ir-pass-trace.md) 使用该功能。

## 打开 Memory Map

右键 `passes_dump` 目录中匹配 `*after_AllocateMemoryAddr.py` 的文件，选择 **PyPTO3 Toolkit：打开内存复用分析器**。也可以在已经打开的 Chip Swimlane 预览面板右上角点击 **openMemoryMap**。

![打开 Memory Map](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/memmap_open.png)

## Buffer 利用率总览

使用 **全部函数 Buffer 使用总览**，可以全局查看所有函数的 Buffer 利用率。

![查看 Buffer 利用率总览](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/memmap_overall.gif)

## 函数内 Tile 分析

单击原 Pass 代码中的函数块，或者从函数总览下拉菜单中选择函数，可以查看该函数内定义的 Tile 占用情况。单击地图中的 Tile 色块，可以查看所选 Tile 的详细信息，以及它在原 Pass 代码中的创建和使用位置。

![分析函数内 Tile](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/memmap_single_func.gif)

## 阅读内存地图

- 横轴表示内存地址；
- 纵轴向下表示生命周期；
- 每个 tile 绘制为一个矩形：横向覆盖该 tile 的 MemRef 所占字节，纵向覆盖 MemRef 存活的语句区间。

通过地址和生命周期两个维度，可以直观看到复用决策，不必在多张表之间来回对照。在函数视图中，纵轴对应每个 Tile 在原 Pass 代码中的生命周期。
