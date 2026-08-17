# 函数性能表

直接打开文件名包含 `name_map` 的 JSON 时，插件读取 `callable_id_to_name`，并结合同目录泳道数据生成函数级性能表。

## 前置条件

`name_map*.json` 所在目录必须至少包含泳道文件：

- `chip_swimlane_records.json`；


示例：

```text
dfx_outputs/
├── chip_swimlane_records.json
└── name_map_<timestamp>.json
```

如果找不到泳道数据，视图会显示错误提示。当前功能不是独立的 Func ID/名称映射浏览器。

## 表格字段

| 字段 | 含义 |
|---|---|
| Func ID | 可调用函数标识 |
| Function Name | `callable_id_to_name` 中的函数名 |
| Count | 函数对应任务的执行次数 |
| Max Duration | 最大执行耗时 |
| Min Duration | 最小执行耗时 |
| Avg Duration | 平均执行耗时 |

耗时单位为微秒。统计会扣除本地 Setup，只计算实际 Kernel 执行时间。表格列支持排序和调整宽度。

## 使用建议

1. 先按 **Avg Duration** 排序，找到持续占用时间较长的函数；
2. 再比较 **Max Duration** 与 **Avg Duration**，识别耗时波动；
3. 结合 **Count** 判断总耗时主要来自单次执行慢，还是执行次数多；
4. 回到 Chip Swimlane 搜索对应函数名，查看具体任务实例和调度上下文。
