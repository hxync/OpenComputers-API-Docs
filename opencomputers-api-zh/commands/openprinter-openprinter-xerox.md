# xerox

## 概述

复制当前由 OpenPrinter 扫描到的页面。

## 可用性

- 仓库：`openprinter`
- 系统：`openprinter`
- 类型：`command`

## 来源

- `OpenPrinter/src\main\resources\assets\openprinter\lua\printer\bin\xerox.lua`

## 语法

```lua
xerox [COUNT]
```

## 用途

先清空打印机缓冲区，扫描当前已装入 OpenPrinter 的页面，再把扫描得到的标题和各行内容重新写回可打印缓冲区，最后按指定份数连续打印同一页。

## 参数

- `COUNT`：要打印的份数。省略时默认为 `1`。只接受 `1` 到 `9` 之间的值。

## 示例

```lua
xerox
```

把当前装入打印机的页面再打印一份。

```lua
xerox 3
```

把当前扫描到的页面连续打印三份。

## 备注

- 脚本直接使用 `component.openprinter`，当机器上存在多台打印机时也不提供地址选择功能。
- 页面标题会先通过 `printer.setTitle(pageTitle)` 重新写回，然后再执行打印。
- 这个命令假定打印机里已经有一页可供扫描和复制的成品页面；它不会从普通文本输入临时生成新文档。
