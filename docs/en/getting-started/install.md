# Install the Extension

PyPTO Toolkit is distributed as a VSIX file attached to each GitHub Release. Always download the extension from the official Releases page and avoid packages from unknown sources.

## Download

1. Open the [latest Release](https://github.com/hw-native-sys/pypto-tools/releases/latest).
2. Under **Assets**, download the file named like `pypto-toolkit-<version>.vsix`.
3. To reproduce an older environment, download the matching version from the [Releases list](https://github.com/hw-native-sys/pypto-tools/releases).

## Install in VS Code

1. Open the **Extensions** view in VS Code.
2. Select `...` in the upper-right corner of the view.
3. Select **Install from VSIX...**.
4. Select the downloaded `.vsix` file.
5. Reload the VS Code window when prompted.

You can also install the extension from the command line:

```bash
code --install-extension /path/to/pypto-toolkit-<version>.vsix
```

## Verify the installation

After installation, verify the following:

- **PyPTO Toolkit** appears in the extension list with the expected version;
- commands starting with `PyPTO Toolkit` appear in the Command Palette;
- right-clicking a supported JSON file or a `passes_dump*` directory in the Explorer shows a PyPTO Toolkit open action.

## Upgrade

Download the new VSIX and repeat the installation steps to overwrite the installed version. Reload the VS Code window after upgrading. To roll back, download the required historical version from the Releases page and install it again.

After an overwrite installation, run **PyPTO.comGraph: Delete Cache DB File** from the Command Palette to remove the old cache. Otherwise, rendering issues may occur.

> **Keep the VS Code display zoom unchanged**
>
> After opening a PyPTO Toolkit feature view, do not change the VS Code display zoom level. Changing it may cause layout or rendering issues.
