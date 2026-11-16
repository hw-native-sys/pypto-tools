# 安装与升级

## 准备

- 仅在由管理员维护的共享 Ascend 主机上安装。安装程序会配置 root 守护进程和
  系统级命令。
- 已有的工具根目录（默认 `/home/pypto-tools/pto-task`）会被保留：升级不会覆盖
  `config/` 和 `state/`。
- 需要 Python 3.9 或更高版本，以及 systemd。

## 下载与校验

1. 打开 [task-submit Release 列表](https://github.com/hw-native-sys/pypto-tools/releases?q=task-submit-v&expanded=true)，
   选择一个 `task-submit-v*` 发布。
2. 下载 `task-submit-<version>.tar.gz` 和 `task-submit-<version>.tar.gz.sha256`。
3. 校验并解压：

```bash
sha256sum -c task-submit-<version>.tar.gz.sha256
tar xzf task-submit-<version>.tar.gz
cd task-submit-<version>
```

## 安装

```bash
sudo bash deploy.sh
```

`deploy.sh` 会安装程序文件、创建缺失的配置、启用 `pto-task.service`、启动守护
进程并报告状态。从新版本目录再次运行它就是受支持的升级方式。

如果只想安装或更新文件、不启动或重启守护进程，使用 `setup.sh`：

```bash
sudo bash setup.sh
```

常用选项：

| 选项 | 用途 |
|---|---|
| `--tools-root DIR` | 安装到其他根目录（默认 `/home/pypto-tools`） |
| `--max-concurrent N` | 最大同时运行任务数 |
| `--available-devices LIST` | 自动分配池，例如 `0,1,2,3` |
| `--task-execution-mode MODE` | `HwHiAiUser`（默认）或 `root` |
| `--ptoas-base DIR` | 已安装 PTOAS 版本所在的根目录 |
| `--non-interactive` | 不提示，使用选项或探测到的默认值 |

首次交互式安装会提示卡数和最大并发数；显式选项优先。

## 验证安装

```bash
systemctl status pto-task.service --no-pager
task-submit --list
task-submit --devices status
```

`--devices status` 会显示自动分配池，以及当前目录下项目策略对它的收窄情况。

## 升级

下载新版本、校验后，在新目录中运行 `sudo bash deploy.sh`。配置和队列状态会保留。
如果有任务正在运行，程序文件会更新但守护进程不会重启；等队列排空后重新运行同一
条命令即可。

主机也可以从源码仓库的 rollout 控制接收更新；该通道在主机上配置，不需要本页的
步骤。

## 回滚

下载较旧的 `task-submit-v*` 发布，校验后在该目录运行 `deploy.sh`。队列状态不会
回滚。

## 卸载

```bash
sudo systemctl disable --now pto-task.service
sudo rm -f /etc/systemd/system/pto-task*.service /etc/systemd/system/pto-task*.timer
sudo rm -f /etc/systemd/system/taskqueue.service
sudo systemctl daemon-reload
sudo rm -f /usr/local/bin/task-submit /usr/local/bin/pto-task /usr/local/bin/pypto-setup
sudo rm -rf /home/pypto-tools/pto-task
```

如果安装时修改过工具根目录，请相应调整路径。删除工具根目录会同时删除本地配置和
队列历史，必要时先备份。

## 故障排查

- **服务未运行。** 运行 `systemctl status pto-task.service --no-pager`，并查看
  `<tools-root>/pto-task/logs` 下的守护进程日志。
- **没有可用设备。** 检查 `task-submit --devices status` 和
  `<tools-root>/pto-task/config/taskqueue.conf` 中的 `--available-devices` 设置。
- **任务看不到设备。** 在任务内通过 `$TASK_DEVICE` 传入分配的设备；见
  [基本使用](usage.md)。

配置和调度细节见
[源码仓库 README](https://github.com/pypto-tools/npu-taskqueue#readme)。
