# mpt

## 概述

从 MPT、OPPM 或镜像后端安装、移除、更新和升级 Plan9k 软件包。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\mpt.lua`

## 语法

```lua
mpt -S PACKAGE...
```
```lua
mpt -R PACKAGE...
```
```lua
mpt -y
```
```lua
mpt -u
```
```lua
mpt [OPTIONS]
```

## 用途

这是 Plan9k 自带的软件包管理器。它把数据库保存在 `/var/lib/mpt` 下，能够同步软件包列表、连同依赖一起安装指定软件包、移除已安装软件、升级过时软件，也支持替代根目录、镜像源和离线数据源等高级工作模式。

## 参数

- `PACKAGE...`: 一个或多个要安装或删除的软件包名称。

## 选项

- `-S, --sync`: 从已配置的远端后端安装指定软件包，并自动拉取所需依赖。
- `-R, --remove`: 删除指定的已安装软件包。
- `-y, --update`: 刷新需要下载索引的后端软件包列表。
- `-u, --upgrades`: 升级当前已安装、但版本落后于活跃后端的软件包。
- `-f, --force`: 强制执行操作。在升级流程中，这也会强制重新下载软件包。
- `--root=PATH`: 使用替代根目录，而不是默认的 `/`。
- `-v`: 输出更详细的日志。
- `-Y`: 在确认提示中自动回答“是”。
- `-r, --reboot`: 软件包操作完成后重启系统。
- `--offline`: 只使用支持离线工作的后端，例如镜像源。
- `--mirror=PATH`: 把另一个根目录当作离线软件包镜像源。
- `--oppmRepos=URL`: 覆盖 OPPM 前端用于读取仓库配置的 URL。
- `-h, --help`: 输出内置帮助文本。

## 示例

```lua
mpt -Sy
```
先刷新软件包索引。

```lua
mpt -S coreutils
```
安装 `coreutils` 软件包，并自动补齐缺失依赖。

```lua
mpt -R coreutils
```
删除已安装的 `coreutils` 软件包。

```lua
mpt -u -Y
```
在不交互确认的情况下升级过时软件包。

## 备注

- 管理器会把安装文件和元数据记录到 `/var/lib/mpt` 中，并且在安装前如果发现文件冲突就会直接终止。
- 删除和升级是按文件级别执行的，因此如果你手工修改过某个软件包安装出的文件，之后可能会遇到冲突或清理结果不符合预期的情况。
