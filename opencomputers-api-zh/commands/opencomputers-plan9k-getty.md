# getty

## 概述

把 Plan9k 伪终端数据流渲染到指定的 GPU 与屏幕组合上。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\getty.lua`

## 语法

```lua
getty GPU_ADDRESS SCREEN_ADDRESS
```

## 用途

这是 Plan9k 使用的终端显示进程。它从标准输入读取字符，解析光标移动、颜色、清屏、滚动以及若干 Plan9k 扩展转义序列，再通过指定 GPU 把结果绘制到屏幕上。

## 参数

- `GPU_ADDRESS`: 用于绘制输出的 GPU 组件地址。
- `SCREEN_ADDRESS`: 要显示内容的屏幕组件地址，通常会与该 GPU 配对使用。

## 示例

```lua
getty 1a2 3b4
```
为地址前缀为 `1a2` 的 GPU 和地址前缀为 `3b4` 的屏幕启动一个显示渲染进程。

## 备注

- 这个程序通常由 Plan9k 的 `init` 自动拉起，并不是给普通用户当作日常交互命令使用的。
- 它除了常见 ANSI 风格转义序列外，还支持 Plan9k 的 `ESC 9` 扩展，用于改分辨率、复制区域和交换终端元数据。
