# Function Performance

When you open a JSON file whose name contains `name_map`, the extension reads `callable_id_to_name` and combines it with swimlane data in the same directory to generate a function-level performance table.

## Prerequisite

The directory containing `name_map*.json` must also contain the following swimlane file:

- `chip_swimlane_records.json`.

Example:

```text
dfx_outputs/
├── chip_swimlane_records.json
└── name_map_<timestamp>.json
```

If no swimlane data is found, the view displays an error. This feature is not a standalone Func ID-to-name mapping browser.

## Table columns

| Column | Meaning |
|---|---|
| Func ID | Callable function identifier |
| Function Name | Function name from `callable_id_to_name` |
| Count | Number of task executions for the function |
| Max Duration | Maximum execution duration |
| Min Duration | Minimum execution duration |
| Avg Duration | Average execution duration |

Durations are measured in microseconds. Statistics subtract local setup time and include only actual kernel execution. You can sort columns and adjust their widths.

## Recommended workflow

1. Sort by **Avg Duration** to find functions with consistently long executions;
2. compare **Max Duration** with **Avg Duration** to identify duration variability;
3. use **Count** to distinguish expensive individual calls from high invocation counts;
4. search for the function name in Chip Swimlane to inspect individual task instances and their scheduling context.
