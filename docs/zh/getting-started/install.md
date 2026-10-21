# 安装插件

PyPTO3 Toolkit 以 VSIX 文件随 GitHub Release 发布。建议始终从官方 Release 页面获取插件，不要使用来源不明的安装包。

## 下载

1. 打开 [最新 Release](https://github.com/hw-native-sys/pypto-tools/releases/latest)。
2. 在 **Assets** 中下载名称类似 `pypto3-toolkit-<version>.vsix` 的文件。
3. 如需复现历史环境，可从 [Releases 列表](https://github.com/hw-native-sys/pypto-tools/releases) 下载对应版本。

## 在 VS Code 中安装

1. 打开 VS Code 的 **Extensions** 视图。
2. 点击视图右上角的 `...`。
3. 选择 **Install from VSIX...**。
4. 选择下载的 `.vsix` 文件。
5. 安装完成后，按提示重新加载 VS Code 窗口。

也可以使用命令行安装：

```bash
code --install-extension /path/to/pypto3-toolkit-<version>.vsix
```

## v1.0.2 与 runtime 的兼容性核对

[v1.0.2 VSIX](https://github.com/hw-native-sys/pypto-tools/releases/tag/v1.0.2) 基于 [PyPTO3 Toolkit 源码提交 `b6ec5589`](https://gitcode.com/majin0824/pypto-tools_pypto3/commit/b6ec5589a85ddbd26131383db7619eb6de86c89a) 打包。已用留存的 `simpler` 主线原始数据检查此包中的转换器：

| `simpler` 提交 | 提交日期 | 已转换的运行数据 |
|---|---|---|
| [`cc6c4d51`](https://github.com/hw-native-sys/simpler/commit/cc6c4d51f79e98af5ef07829e3ca71c36f752a43) | 2026-10-15 | TensorMap and RingBuffer、Host Build Graph |
| [`545c05a4`](https://github.com/hw-native-sys/simpler/commit/545c05a43469a7fd87d8f8eff2dafdf22922a796) | 2026-10-21 | Host Build Graph |

截至 2026-10-21，已检查的最新 runtime 提交为 `545c05a4`。上述日期只是样本核对的时间跨度，不代表期间每个提交或每个 IDE 视图都经过验证。使用其他 runtime 提交时，请在 IDE 中打开有代表性的 `chip_swimlane_records.json`，核对格式和展示。

此 Release 的 GitHub 自动生成的 **Source code** 压缩包包含的是本说明文档仓库；插件源码请使用上方链接的 GitCode 提交。

## 验证安装

安装后可进行以下检查：

- 在扩展列表中能找到 **PyPTO3 Toolkit**，并可以确认版本号是否正确；
- 打开命令面板后，能够搜索到以 `PyPTO3 Toolkit` 开头的命令；
- 在资源管理器中右键受支持的 JSON 文件或 `passes_dump*` 目录时，能够看到 PyPTO3 Toolkit 的打开入口。

## 升级

下载新版本 VSIX 后，重复安装步骤即可覆盖升级。升级后建议重新加载 VS Code 窗口。需要回退时，从 Releases 页面下载目标历史版本并重新安装。

覆盖安装后，请在命令面板中执行 **PyPTO.comGraph: Delete Cache DB File** 清理旧版本缓存，否则可能出现渲染异常。

> **注意：保持 VS Code 显示比例不变**
>
> 打开 PyPTO3 Toolkit 功能视图后，不建议调整 VS Code 窗口的显示缩放比例，否则可能出现视图布局或渲染异常。
