# update

## 概述

从已配置的发布树，或手动指定的更新树中更新 SecureOS。

## 可用性

- 仓库：`opensecurity`
- 系统：`secureos`
- 类型：`command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\update.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\update`

## 语法

```lua
update
```

```lua
update [-a] TREE
```

## 用途

要求 root 权限和网络卡，根据命令行参数或 `/etc/update.cfg` 选定更新树，下载远程更新脚本和废弃包列表，移除过期文件，执行下载到本地的更新器，最后重启机器。

## 参数

- `TREE`：远程更新树名称，只能是 `release` 或 `dev`。

## 选项

- `-a`：把所选更新树写入 `/etc/update.cfg`，作为之后更新的默认值。

## 示例

```lua
update
```

使用 `/etc/update.cfg` 中当前记录的更新树执行更新。

```lua
update dev
```

拉取一次开发树，并在更新后重启进入新系统。

```lua
update -a release
```

把记忆中的更新树切换为 `release`，按它更新，并在下次继续使用它。

## 备注

- 如果当前用户不是 root，或机器没有可用的 `internet` 组件，命令会直接拒绝执行。
- 运行过程中会把 `update-tmp.lua`、`update-tmp.cfg` 和 `depreciated.dat` 等临时文件下载到 `/tmp`。
- 更新成功后，它会清理 `/tmp/.root`，然后调用 `computer.shutdown(true)` 立即重启。
