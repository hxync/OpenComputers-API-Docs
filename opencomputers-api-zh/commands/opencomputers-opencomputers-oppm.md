# oppm

## 概述

在安装介质上引导 OpenPrograms 包管理器，并在隐藏专家模式下执行其安装器功能。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\oppm\usr\bin\oppm.lua`

## 语法

```lua
oppm
```
```lua
oppm -iKnowWhatIAmDoing list [-i] [FILTER]
```
```lua
oppm -iKnowWhatIAmDoing info PACKAGE
```
```lua
oppm -iKnowWhatIAmDoing install [-f] PACKAGE [PATH]
```
```lua
oppm -iKnowWhatIAmDoing update PACKAGE|all
```
```lua
oppm -iKnowWhatIAmDoing uninstall PACKAGE
```

## 用途

仓库里自带的这个 `oppm` 脚本并不是正常安装后的完整包管理器。默认情况下，它只会输出警告并要求你去执行 `/bin/install.lua`。只有显式传入隐藏的 `-iKnowWhatIAmDoing` 开关后，它才会启用这个引导版实现，允许列包、查看信息、安装、更新和卸载软件包。

## 参数

- `FILTER`: 可选，`list` 子命令使用的子串过滤条件。
- `PACKAGE`: 要查看、安装、更新或卸载的软件包名称。
- `PATH`: 可选，安装目标目录。默认使用配置中的路径，或 `/usr`。

## 选项

- `-iKnowWhatIAmDoing`: 解锁这个安装器脚本内部隐藏的引导版包管理功能。
- `-i`: 与 `list` 配合时，只显示本地记录为已安装的软件包。
- `-f`: 安装时强制创建目录，并允许覆盖已有文件。
- `--nocache`: 关闭当前运行过程中的仓库和软件包列表缓存。

## 示例

```lua
oppm
```
显示安装器警告，提示你应当改为执行 `/bin/install.lua`。

```lua
oppm -iKnowWhatIAmDoing list
```
使用引导版客户端列出配置好的 OpenPrograms 仓库中全部可见软件包。

```lua
oppm -iKnowWhatIAmDoing install mineos /usr
```
使用引导版实现把 `mineos` 软件包安装到 `/usr`。

## 备注

- 这个脚本会把已安装软件包元数据写入 `/etc/opdata.svd`，并把本地仓库覆盖配置保存在 `/etc/oppm.cfg` 中。
- 如果软件包声明里的文件路径以 `:` 开头，脚本会先调用 GitHub contents API 展开整个目录，再逐个下载文件。
