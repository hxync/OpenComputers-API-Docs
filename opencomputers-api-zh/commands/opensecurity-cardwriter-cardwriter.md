# cardwriter

## 概述

以交互方式把数据、显示名称和锁定标志写入 OpenSecurity 磁条卡或 RFID 卡。

## 可用性

- 仓库: `opensecurity`
- 系统: `cardwriter`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\cardwriter\bin\cardwriter.lua`

## 语法

```lua
cardwriter
```

## 用途

先清空终端，再通过交互提示依次收集三项信息，然后调用 `os_cardwriter` 组件把它们写入当前插入的卡片：卡片原始数据、卡片显示名称，以及写入后是否立刻锁卡。

## 交互字段

- `Card Data`: 要存入卡片中的原始数据字符串。
- `Card Name`: 写入卡片的显示名称。
- `Lock Card`: 写入完成后是否立刻锁定卡片，阻止后续修改。

## 示例

```lua
cardwriter
```
打开交互式写卡提示，并通过已连接的 OpenSecurity 写卡器写入一张卡片。

## 备注

- 这个命令完全依赖交互输入，不接受命令行参数。
- 实现内部通过 `component.os_cardwriter` 写卡，因此机器上必须存在这个组件。
- 虽然提示文本写的是 `[y/N]`，但实现里直接按回车也会锁卡，因为空输入会被当成“是”。
- 写入完成后，脚本会再次清空终端，而不会额外输出成功摘要。
