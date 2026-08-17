# Chip Swimlane

Chip Swimlane shows the timeline of PyPTO 3.0 tasks from scheduling through AICore execution. The extension reads `chip_swimlane_records.json` directly; you do not need to convert it to Chrome Trace format first.

## Open a swimlane

In the VS Code Explorer, right-click the swimlane JSON file and select **PyPTO Toolkit: Open File**.

![Open Chip Swimlane](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/1_chip_swim_open.gif)

The extension creates views based on the data actually present in the input file:

| View | Description |
|---|---|
| Worker View | Shows task start time, end time, and duration on each AIC/AIV core |
| Scheduler View | Shows AICPU scheduling intervals from dispatch to finish |
| Scheduler Phase | Shows phases such as dispatch, early_dispatch, wire, release, resolve, and drain |
| Orchestrator Phase | Shows orchestrator submission phases and their relationship with the scheduler |

A low record level does not infer higher-level scheduling data. For example, a file containing only worker tasks does not produce a complete Scheduler Phase view.

## Inspect task details and dependencies

Select a task to display its timing, Ring/Task ID, function information, setup duration, fan-out, and other details in the lower panel. When `deps.json` exists in the same directory, the view also displays task dependency lines.

![Inspect task details](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/2_chip_swim_click_task.gif)

You can configure the dependency depth:

- `0`: do not expand dependencies;
- a positive integer: show the specified number of predecessor and successor levels;
- refer to the current extension interface for the available range.

Selecting one SPMD task highlights tasks with the same name. On a dependency path, identically named SPMD tasks are consolidated to prevent excessive lines from obscuring the view.

## Search for tasks

Enter a function name or Task ID in the search box to locate tasks with a fuzzy match.

![Search for a task](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/3_chip_swim_search.gif)

Task labels can show the function name, `Ring & Task ID`, or both.

## Time markers

You can add markers on the time axis. You can also right-click a task to draw markers at its start and end, or at the overall boundaries of identically named SPMD tasks. Each marker displays its timestamp. Select a marker again to close its action panel.

![Add a time marker](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/5_chip_swim_set_line.gif)

![Mark SPMD task boundaries](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/6_chip_swim_set_spmd_line.gif)

## Performance panel

Select **Performance Statistics** in the upper-right corner to inspect duration and setup statistics. Selecting a task in the statistics locates the corresponding record in the swimlane.

![Performance panel](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/7_chip_swim_perf.png)

## Setup analysis

For records that include receive-to-start data, the extension treats the interval from receive to start as local setup:

- renders setup as a separate region in each worker task block;
- lets you configure whether the total duration includes setup;
- lets you hide setup rendering;
- displays setup duration in task details;
- reports tasks with the longest setup and the largest setup ratio in the performance panel;
- excludes setup from function-level kernel duration statistics.

![Configure setup display](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/9_chip_swim_show_setup.gif)

## Critical-path highlighting

When `CPM_static*.json` or `CPM_observed*.json` exists in the same directory, you can select a critical path in the rendering settings. Tasks outside the selected critical path are de-emphasized so you can focus on the main execution chain.

![Highlight a critical path](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/10_chip_swim_show_CPM.gif)

> **Important**
>
> The extension only reads and highlights existing CPM results. It does not calculate or generate a critical-path report in the IDE.

## Other operations

- Hover over the left side of a lane to pin that lane to the top;
- export the current swimlane as a PNG image;
- use the top navigation action to open an associated IR Pass Trace found in the current directory, its parent, or a `passes_dump*` directory.

| Operation | Function |
|---|---|
| Mouse wheel | Move vertically |
| `Ctrl` + left-button drag | Move horizontally |
| `Ctrl` + mouse wheel | Zoom |
| `Alt` + left mouse button | Measure a time interval manually |
| `w` / `s` | Zoom in/out when enabled by the interface configuration |
| `a` / `d` | Move horizontally when enabled by the interface configuration |
