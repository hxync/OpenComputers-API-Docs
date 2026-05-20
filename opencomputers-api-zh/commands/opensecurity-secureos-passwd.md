# passwd

## 概述

修改当前已登录 SecureOS 账户的密码。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\passwd.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\passwd`

## 语法

```lua
passwd
```

## 用途

先提示输入当前密码，再要求两次输入新密码。如果当前密码校验成功，且两次新密码输入一致，SecureOS 会通过 `auth.addUser` 重写该用户记录，并保留账户现有的超级用户标志。

## 示例

```lua
passwd
```
为当前账户打开交互式 SecureOS 改密提示。

## 备注

- 如果旧密码错误，或两次新密码输入不一致，脚本会输出失败消息，并保持账户不变。
