# readkey

## 概述

把键盘与剪贴板事件转换成屏幕会话可消费的终端输入字节流。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\readkey.lua`

## 语法

```lua
readkey SCREEN_ADDRESS
```

## 用途

监听当前连接到指定屏幕的所有键盘，把普通字符直接写到标准输出，并把方向键、功能键、Delete、Home、PageUp 和 PageDown 等特殊按键编码为转义序列。

## 参数

- `SCREEN_ADDRESS`: 要监听其附属键盘的屏幕组件地址。

## 示例

```lua
readkey 3b4
```
把地址前缀为 `3b4` 的屏幕所连接键盘事件转换成标准输入风格的字节流。

## 备注

- 这个辅助进程通常会和 `getty` 及 shell 一起组成 Plan9k 的伪终端会话。
- 它只会记录进程启动时已经连接到该屏幕的键盘。
