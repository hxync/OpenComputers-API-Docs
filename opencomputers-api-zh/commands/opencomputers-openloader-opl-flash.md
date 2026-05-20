# opl-flash

## 概述

把内置的 OpenLoader 引导程序刷写进当前安装的 EEPROM。

## 可用性

- 仓库: `opencomputers`
- 系统: `openloader`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openloader\bin\opl-flash.lua`

## 语法

```lua
opl-flash [-hqr] [--label=EEPROM_LABEL]
```

## 用途

把内置的 OpenLoader EEPROM 镜像写入当前 EEPROM 组件，并可选地设置标签或在完成后立刻重启。

## 参数

- `EEPROM_LABEL`: 可选，刷写完成后要设置的 EEPROM 标签。省略时使用内置的 OpenLoader 版本字符串。

## 选项

- `-q, --quiet`: 不打印提示或状态输出，也不再询问确认。
- `-r, --reboot`: 刷写成功后立刻重启。
- `--label=EEPROM_LABEL`: 刷写完成后设置 EEPROM 标签。
- `-h, --help`: 输出内置帮助文本。

## 示例

```lua
opl-flash
```
先提示确认，再使用默认版本标签刷写 OpenLoader。

```lua
opl-flash --label=GTNH -r
```
刷写 OpenLoader，把 EEPROM 标签设为 `GTNH`，并在完成后立刻重启。

## 备注

- 安静模式意味着非交互确认，命令会直接开始刷写。
