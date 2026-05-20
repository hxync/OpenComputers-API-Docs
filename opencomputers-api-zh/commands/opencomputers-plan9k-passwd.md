# passwd

## 概述

创建或替换保存在 `/etc/passwd` 中的 Plan9k 登录密码哈希。

## 可用性

- 仓库：`opencomputers`
- 系统：`plan9k`
- 类型：`command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\passwd.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\passwd`

## 语法

```lua
passwd
```

## 用途

两次提示输入新密码，通过数据卡生成一个 16 字节随机密钥，再用 `component.data.sha256` 对密码做哈希，并把打包后的“随机密钥 + 哈希值”二进制块以 Base64 形式写入 `/etc/passwd`。

## 示例

```lua
passwd
```

交互式输入新的 Plan9k 密码，并把带随机密钥的哈希保存到 `/etc/passwd`。

## 备注

- 实现依赖同时提供 `random` 和 `sha256` 的数据卡；没有这类数据卡时，密码存储无法可靠完成。
- 当前实现不会先要求输入旧密码。
- 当前实现即使打印了 `Passwords do not match`，仍会继续尝试改写 `/etc/passwd`；如果确认输入不一致，应立即重新执行该命令。
