# clear

## 概述

清空终端内容，并把光标重置到左上角。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\clear.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\clear`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\clear`

## 语法

```lua
clear
```

## 用途

使用当前前景色和背景色清除整块屏幕上的文本内容，然后把光标重新放回 `(1, 1)` 位置。

## 示例

```lua
clear
```
在重新绘制一屏新输出之前清空终端。
