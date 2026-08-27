# Serving Strace Swimlane

Serving Strace Swimlane displays the Serving task timeline recorded in `serving-strace-swimlane.json` and aggregates task performance by `WorkerProcess`.

## Open a swimlane

In the VS Code Explorer, right-click `serving-strace-swimlane.json` and select **PyPTO Toolkit: Open File**.

![Open Serving Strace Swimlane](https://raw.githubusercontent.com/hw-native-sys/pypto-tools/main/.image/serving-strace-swimlane_open.gif)

## Inspect performance statistics

The performance panel reports the following metrics for each `WorkerProcess`:

- task count;
- average duration;
- maximum duration;
- minimum duration.

Use these statistics to compare task volume and duration distributions across worker processes.
