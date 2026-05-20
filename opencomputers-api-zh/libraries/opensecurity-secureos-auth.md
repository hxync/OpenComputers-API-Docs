# auth

## 概述

SecureOS 的密码数据库辅助库，用于账户创建、密码校验、权限检查和审计日志记录。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\auth.lua`

## 用法

```lua
local auth = require("auth")
```

## 接口

### auth.addUser(username, password, su)

- 说明: 添加或覆盖一条密码数据库记录，对给定密码做哈希，并记录该账户是否为超级用户。
- 参数:
  - `username`
  - `password`
  - `su`

```lua
auth.addUser("value", nil, nil)
```

### auth.isRoot()

- 说明: 返回当前保存在 `/tmp/.root` 中的临时 root 标记；如果不存在则返回 `false`。

```lua
auth.isRoot()
```

### auth.rmUser(username)

- 说明: 从密码数据库中移除一个用户条目，并重写 passwd 文件。
- 参数:
  - `username`

```lua
auth.rmUser("value")
```

### auth.userLog(username, arg)

- 说明: 在根文件系统可写时，向 `/var/log/auth.log` 追加一条认证审计记录。
- 参数:
  - `username`
  - `arg`

```lua
auth.userLog("value", nil)
```

### auth.validate(username, password)

- 说明: 检查提供的密码是否匹配某个用户的已存储哈希，并同时返回该用户是否被标记为超级用户。
- 参数:
  - `username`
  - `password`

```lua
auth.validate("value", nil)
```

## 备注

- 本页只列出模块导出的公开函数。
