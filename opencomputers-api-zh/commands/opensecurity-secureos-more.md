# more

## 概述

按终端页高分页查看文本文件。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\more.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\more`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\more`

## 语法

```lua
more FILE
```

## 用途

逐行读取目标文件，并按当前 GPU 宽度自动换行后分页显示。每页显示完后会等待键盘输入：`Space` 或 `PageDown` 显示下一页，`Enter` 或向下键继续显示一行，`q` 退出。

## 参数

- `FILE`: 要显示的文件路径。

## 示例

```lua
more /home/a.txt
```
按页显示 `/home/a.txt` 的内容。

## 备注

- 这个实现不支持向前翻回已经看过的内容。
- 长行会通过 `text.wrap` 自动换行，因此在窄屏上可能更快翻页。
