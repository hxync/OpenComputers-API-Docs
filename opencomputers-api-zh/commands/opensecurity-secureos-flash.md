# flash

## 概述

读取、保存或重写当前安装的 EEPROM 内容。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\flash.lua`

## 语法

```lua
flash -l
```
```lua
flash -r FILE
```
```lua
flash [-q] BIOS_FILE [LABEL]
```

## 用途

管理当前安装的 EEPROM。你可以打印当前 ROM 内容、把它保存到文件，或者把某个 BIOS 文件刷写进 EEPROM，并且可选地更新标签。

## 参数

- `FILE`: 在配合 `-r` 读取当前 EEPROM 内容时，用来保存结果的目标文件。
- `BIOS_FILE`: 要写入 EEPROM 的二进制或 Lua BIOS 镜像文件。
- `LABEL`: 可选，新 EEPROM 标签。如果在交互模式下省略，命令会提示输入。

## 选项

- `-q`: 安静模式。跳过确认提示和状态输出。
- `-l`: 把当前 EEPROM 内容输出到标准输出。
- `-r`: 读取当前 EEPROM 内容并保存到文件。

## 示例

```lua
flash -l
```
输出当前已安装 EEPROM 的镜像内容。

```lua
flash -r backup.lua
```
把当前 EEPROM 内容保存到 `backup.lua`。

```lua
flash bios.lua MyBIOS
```
把 `bios.lua` 写入 EEPROM，并将标签设为 `MyBIOS`。

## 备注

- 交互式写入模式会在刷写前提示确认，并明确警告操作期间不要断电。
- 使用 `-r` 且目标文件已存在时，非安静模式会先询问是否覆盖。
