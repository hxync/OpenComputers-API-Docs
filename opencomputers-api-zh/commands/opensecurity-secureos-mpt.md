# mpt

## 概述

使用 SecureOS 自带的 MPT 管理器安装、移除和升级软件包。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\mpt.lua`

## 语法

```lua
mpt -S PACKAGE...
```
```lua
mpt -R PACKAGE...
```
```lua
mpt -u
```
```lua
mpt [OPTIONS]
```

## 用途

SecureOS 附带了一份自己的 `mpt` 变体。它把软件包状态保存到 `/var/lib/mpt` 下，能从配置好的 MPT 后端安装指定软件包及其依赖、删除已安装软件包、升级过时软件包，也支持替代根目录。

## 参数

- `PACKAGE...`: 一个或多个要安装或删除的软件包名称。

## 选项

- `-S, --sync`: 安装指定软件包及其依赖。
- `-R, --remove`: 删除指定的已安装软件包。
- `-u, --upgrades`: 升级版本落后于后端仓库的软件包。
- `-f, --force`: 在软件包操作中强制重新下载并允许覆盖。
- `--root=PATH`: 使用替代根目录，而不是默认的 `/`。
- `-v`: 启用更详细的日志输出。
- `-y`: 在确认提示中自动回答“是”。
- `-r, --reboot`: 软件包操作完成后重启。
- `-h, --help`: 输出内置帮助文本。

## 示例

```lua
mpt -S opensecurity
```
安装 `opensecurity` 软件包，并补齐缺失依赖。

```lua
mpt -R opensecurity
```
删除已安装的 `opensecurity` 软件包。

```lua
mpt -u -y
```
不经过交互确认直接升级过时软件包。

## 备注

- 和 Plan9k 版本相比，这个 SecureOS 实现没有暴露包列表更新、镜像源或 OPPM 前端相关选项。
