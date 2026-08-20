# IR Pass Trace

IR Pass Trace compares consecutive Python IR snapshots before and after compiler passes. It helps identify which functions a pass changed and whether the pass generated warnings.

## Input requirements

The directory name must start with `passes_dump` and follow these conventions:

```text
passes_dump_<timestamp>/
├── 00_frontend.py
├── 01_PassA.py
├── 01_PassA.log       # optional
├── 02_PassB.py
└── 03_PassC.py
```

- `00_frontend.py` must exist;
- subsequent snapshots must follow `NN_<PassName>.py`;
- pass numbers must start at 1 and remain consecutive, with no missing or duplicate numbers;
- a matching `.log` file is optional and displays warnings for that pass.

Each pass compares the previous snapshot with the current pass snapshot. For example, `02_PassB.py` is compared with `01_PassA.py`.

## Open IR Pass Trace

In the VS Code Explorer, right-click a `passes_dump*` directory and select IR Pass Trace. You can also navigate from the top of Chip Swimlane to an associated directory found by the extension.

![IR Pass Trace](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/pass_IR_trace.gif)

## Inspect changes

The left pane lists passes by number and reports added and removed line counts. The view supports:

- marking No-op passes and filtering to Changed or No-op passes;
- marking and displaying warnings from matching `.log` files;
- selecting the first changed pass by default;
- filtering differences by top-level function or class method;
- displaying both line-level and character-level differences;
- switching between Side by side and Stacked layouts;
- synchronized scrolling in the two-column layout;
- folding large unchanged sections and expanding individual or all sections;
- using `j`/`k` or the arrow keys to move between passes.

## Recommended workflow

1. Enable the **Changed** filter to find passes that actually modify the IR;
2. filter by function to focus on the current operator entry point or a suspicious function.

For an interactive view of on-chip memory reuse in a pass dump, see [Memory Reuse Analysis](memory-reuse.md).

> **Note**
>
> IR Pass Trace is a read-only viewer. It does not edit or write back to files in `passes_dump`.
