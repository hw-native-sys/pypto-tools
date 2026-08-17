# 安装插件

PyPTO Toolkit 以 VSIX 文件随 GitHub Release 发布。建议始终从官方 Release 页面获取插件，不要使用来源不明的安装包。

## 下载

1. 打开 [最新 Release](https://github.com/hw-native-sys/pypto-tools/releases/latest)。
2. 在 **Assets** 中下载名称类似 `pypto-toolkit-<version>.vsix` 的文件。
3. 如需复现历史环境，可从 [Releases 列表](https://github.com/hw-native-sys/pypto-tools/releases) 下载对应版本。

## 在 VS Code 中安装

1. 打开 VS Code 的 **Extensions** 视图。
2. 点击视图右上角的 `...`。
3. 选择 **Install from VSIX...**。
4. 选择下载的 `.vsix` 文件。
5. 安装完成后，按提示重新加载 VS Code 窗口。

也可以使用命令行安装：

```bash
code --install-extension /path/to/pypto-toolkit-<version>.vsix
```

## 验证安装

安装后可进行以下检查：

- 在扩展列表中能找到 **PyPTO Toolkit**，并可以确认版本号是否正确；
- 打开命令面板后，能够搜索到以 `PyPTO Toolkit` 开头的命令；
- 在资源管理器中右键受支持的 JSON 文件或 `passes_dump*` 目录时，能够看到 PyPTO Toolkit 的打开入口。

## 升级

下载新版本 VSIX 后，重复安装步骤即可覆盖升级。升级后建议重新加载 VS Code 窗口。需要回退时，从 Releases 页面下载目标历史版本并重新安装。

覆盖安装后，请在命令面板中执行 **PyPTO.comGraph: Delete Cache DB File** 清理旧版本缓存，否则可能出现渲染异常。

> **注意：保持 VS Code 显示比例不变**
>
> 打开 PyPTO Toolkit 功能视图后，不建议调整 VS Code 窗口的显示缩放比例，否则可能出现视图布局或渲染异常。
