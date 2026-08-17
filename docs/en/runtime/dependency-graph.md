# Task Dependency Graph

The Task Dependency Graph reads `tasks`, `edges`, and `tensors` from `deps.json` and displays the PyPTO 3.0 task DAG. The extension removes duplicate edges and attempts to load the newest `name_map*.json` in the same directory to display function names.

## Open the graph

In the VS Code Explorer, right-click the file named exactly `deps.json` and select **PyPTO Toolkit: Open File**.

## Node information

Different shapes and colors distinguish AIC, AIV, MIX, ALLOC, and SPMD tasks. Nodes and the details panel can display:

- function name and Ring/Task ID;
- `block_num` and predecessor and successor counts;
- tensor argument index and input/output type;
- tensor name, dtype, shape, stride, and offset;
- predecessors, successors, and consolidated counts for identically named SPMD tasks.

## Search and dependency highlighting

Search by function name or Task ID to locate a task. Hover over or select a node to highlight its predecessors and successors.

Dependency highlight depth:

| Value | Meaning |
|---:|---|
| `0` | Disable dependency highlighting |
| Positive integer | Expand the specified number of dependency levels |
| `-1` | Expand all transitive dependencies |

The graph supports left-to-right and top-to-bottom layouts, zooming, panning, and Fit to Screen. The HUD shows the node count and the number of currently visible edges.

## Dependency edge modes

| Mode | Meaning |
|---|---|
| Full | Show all dependency edges |
| Reduced | Hide transitively redundant edges |
| Omitted | Show only edges classified as redundant |
| Reduced DF | Hide redundant edges according to Data Flow rules |
| Omitted DF | Show only edges classified as redundant by Data Flow rules |

Start with **Full** to verify the original dependencies. Then use **Reduced** or **Reduced DF** to focus on required dependencies. Switch to the corresponding Omitted mode to inspect the source of redundant edges.

## Early Dispatch markers

Markers in the dependency graph describe dependency metadata or graph structure:

| Marker | Current meaning |
|---|---|
| `🔥` | The task is ALLOC, or it has an `early_dispatch` marker in `deps.json` |
| `⭐` | All predecessors belong to the early-processable set, and at least one predecessor is not ALLOC |

> **Important**
>
> These markers do not prove that the task was actually dispatched early during this run. The dependency graph cannot report a runtime ratio such as “N of M physical blocks were dispatched early.” An `early_dispatch` phase in the Scheduler swimlane only shows that the phase appears in the runtime record.
