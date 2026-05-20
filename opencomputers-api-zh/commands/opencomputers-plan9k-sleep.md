# sleep

## 概述

按一个或多个时间间隔暂停执行。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\sleep.lua`

## 语法

```lua
sleep NUMBER[SUFFIX]...
```

## 用途

按所有给定时间片段的总和暂停执行。每个时间片段都可以使用秒、分钟、小时或天作为单位，并且接受小数值。

## 参数

- `NUMBER[SUFFIX]...`: 一个或多个非负时间间隔。支持后缀 `s`、`m`、`h`、`d`；省略后缀时按秒处理。

## 选项

- `--help`: 输出内置帮助文本。

## 示例

```lua
sleep 5
```
暂停五秒。

```lua
sleep 1.5m 2s
```
总共暂停一分三十二秒。

## 备注

- 非法选项和负数时长都会被拒绝，并返回用法错误。
- 如果收到 `interrupted` 信号，等待会提前结束。
