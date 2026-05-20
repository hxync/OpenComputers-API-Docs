# explode

## 概述

为自毁卡或服务器自毁器点燃引信，并提供确认与多种倒计时显示模式。

## 可用性

- 仓库：`computronics`
- 系统：`computronics`
- 类型：`command`

## 来源

- `Computronics/src\main\resources\assets\computronics\loot\explode\usr\bin\explode.lua`

## 语法

```lua
explode [-s|-t] [-y] TIME
```

## 用途

校验引信时间，按需要要求用户确认，启动自毁计时器，然后根据选项决定保持静默、在当前 shell 行内刷新倒计时，或切换到专门的全屏倒计时界面直至爆炸。

## 参数

- `TIME`：引信秒数，必须大于 `0` 且不超过 `100000`。

## 选项

- `-s`：静默启动引信，不显示任何倒计时。
- `-t`：保留当前 shell，并在当前行内更新倒计时。
- `-y`：跳过确认提示。

## 示例

```lua
explode 10
```

先要求确认，然后启动 10 秒引信，并切换到全屏倒计时界面。

```lua
explode -t 30
```

启动 30 秒引信，并在当前 shell 中以内联方式显示倒计时。

```lua
explode -s -y 5
```

不询问确认，也不显示倒计时，立即点燃一个 5 秒引信。

## 备注

- `-s` 和 `-t` 互斥，命令会在点火前拒绝同时使用这两个选项。
- 机器上必须存在 `self_destruct` 组件，或者 `server_destruct` 组件。
- 在非静默模式下，引信会先被点燃，然后才检查 GPU 和屏幕是否可用于显示倒计时；因此即使终端不可用，点火本身仍然已经成功。
