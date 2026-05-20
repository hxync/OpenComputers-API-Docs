# logout

## 概述

注销当前 SecureOS 会话并返回登录程序。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\logout.lua`

## 语法

```lua
logout
```

## 用途

如果自动运行仍然启用则先关闭它，然后通过 `auth` 写入注销日志，删除临时 root 标记，把工作目录重置为 `/`，最后启动 `/boot/z_login.lua`。

## 示例

```lua
logout
```
结束当前 SecureOS shell 会话，并返回登录界面。

## 备注

- 命令会删除 `/tmp/.root`，因此存放在这里的临时 root 提权标记也会一并清除。
