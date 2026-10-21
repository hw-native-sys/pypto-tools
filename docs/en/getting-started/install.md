# Install the Extension

PyPTO3 Toolkit is distributed as a VSIX file attached to each GitHub Release. Always download the extension from the official Releases page and avoid packages from unknown sources.

## Download

1. Open the [latest Release](https://github.com/hw-native-sys/pypto-tools/releases/latest).
2. Under **Assets**, download the file named like `pypto3-toolkit-<version>.vsix`.
3. To reproduce an older environment, download the matching version from the [Releases list](https://github.com/hw-native-sys/pypto-tools/releases).

## Install in VS Code

1. Open the **Extensions** view in VS Code.
2. Select `...` in the upper-right corner of the view.
3. Select **Install from VSIX...**.
4. Select the downloaded `.vsix` file.
5. Reload the VS Code window when prompted.

You can also install the extension from the command line:

```bash
code --install-extension /path/to/pypto3-toolkit-<version>.vsix
```

## v1.0.2 runtime compatibility

The [v1.0.2 VSIX](https://github.com/hw-native-sys/pypto-tools/releases/tag/v1.0.2) was built from [the PyPTO3 Toolkit source commit `b6ec5589`](https://gitcode.com/majin0824/pypto-tools_pypto3/commit/b6ec5589a85ddbd26131383db7619eb6de86c89a). Its raw-data converter was checked with retained `simpler` `main` captures:

| `simpler` commit | Commit date | Captures converted |
|---|---|---|
| [`cc6c4d51`](https://github.com/hw-native-sys/simpler/commit/cc6c4d51f79e98af5ef07829e3ca71c36f752a43) | 15 October 2026 | TensorMap and RingBuffer; Host Build Graph |
| [`545c05a4`](https://github.com/hw-native-sys/simpler/commit/545c05a43469a7fd87d8f8eff2dafdf22922a796) | 21 October 2026 | Host Build Graph |

`545c05a4` is the latest checked runtime commit as of 21 October 2026. The dates show a sample check span, not a guarantee for every intervening commit or every IDE view. For another runtime revision, open a representative `chip_swimlane_records.json` in the IDE to check its format and display.

The GitHub-generated **Source code** archives for this Release contain this documentation repository. Use the linked GitCode commit for the extension source.

## Verify the installation

After installation, verify the following:

- **PyPTO3 Toolkit** appears in the extension list with the expected version;
- commands starting with `PyPTO3 Toolkit` appear in the Command Palette;
- right-clicking a supported JSON file or a `passes_dump*` directory in the Explorer shows a PyPTO3 Toolkit open action.

## Upgrade

Download the new VSIX and repeat the installation steps to overwrite the installed version. Reload the VS Code window after upgrading. To roll back, download the required historical version from the Releases page and install it again.

After an overwrite installation, run **PyPTO.comGraph: Delete Cache DB File** from the Command Palette to remove the old cache. Otherwise, rendering issues may occur.

> **Keep the VS Code display zoom unchanged**
>
> After opening a PyPTO3 Toolkit feature view, do not change the VS Code display zoom level. Changing it may cause layout or rendering issues.
