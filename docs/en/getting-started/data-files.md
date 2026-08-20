# Prepare Analysis Files

PyPTO Toolkit does not collect or generate PyPTO runtime records. It reads files already produced by the framework and visualizes them. More complete input data enables more detailed views.

## Runtime files

| File | Opens independently | Purpose |
|---|---:|---|
| `chip_swimlane_records.json` | Yes | Provides task, core, scheduler, orchestrator, and timing information |
| `deps.json` | Yes | Provides the task DAG, tensor information, and swimlane dependency lines |
| `name_map*.json` | Conditional | Maps Func IDs to function names and generates function performance data with a swimlane file |
| `CPM_static*.json` | No | Provides tasks from an existing static critical-path result |
| `CPM_observed*.json` | No | Provides tasks from an existing observed critical-path result |

Recommended layout:

```text
dfx_outputs/
├── chip_swimlane_records.json
├── deps.json
├── name_map_<timestamp>.json
├── CPM_static.json             # optional
└── CPM_observed.json           # optional
```

When you open a swimlane file, the extension looks for `deps.json` and `name_map*.json` in the same directory:

```text
chip_swimlane_records.json
├── deps.json       → task dependencies, kernel_ids, and Func IDs
└── name_map*.json  → Func ID-to-function-name mapping
```

Missing related files have the following effects:

- without `deps.json`, the extension cannot fully recover Func IDs or display data-dependency lines;
- without `name_map*.json`, some tasks display only IDs instead of function names;
- without CPM files, the corresponding critical-path highlight mode is unavailable;
- with only `name_map*.json` and no swimlane file, the function performance view cannot calculate durations.

## IR pass snapshots

IR Pass Trace requires a directory whose name starts with `passes_dump` and contains consecutively numbered Python snapshots:

```text
passes_dump_<timestamp>/
├── 00_frontend.py
├── 01_PassA.py
├── 01_PassA.log       # optional; displays warnings for this pass
├── 02_PassB.py
└── 03_PassC.py
```

The following constraints apply:

- `00_frontend.py` must exist;
- subsequent filenames must follow `NN_<PassName>.py`;
- pass numbering must start at 1 and remain consecutive, with no missing or duplicate numbers;
- a matching `.log` file is optional.

The same pass dump can also be processed by `memory_map` for [Memory Reuse Analysis](../compiler/memory-reuse.md). The analysis uses the pass dump to show the address range and lifetime of each MemRef.
