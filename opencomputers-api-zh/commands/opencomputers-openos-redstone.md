# redstone

## 概述

查看并控制普通、捆绑式或无线红石信号。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\redstone.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\redstone`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\de_DE\block\redstone.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\en_US\block\redstone.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\fr_FR\block\redstone.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\ru_RU\block\redstone.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\zh_CN\block\redstone.md`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\redstone`

## 语法

```lua
redstone SIDE [VALUE]
```
```lua
redstone -b SIDE COLOR [VALUE]
```
```lua
redstone -w [VALUE]
```
```lua
redstone -f [FREQUENCY]
```

## 用途

通过 shell 使用红石卡或红石 I/O 方块。根据所用标志，命令既可以读取或设置普通侧面输出，也可以操作捆绑线颜色输出、无线红石布尔输出，或查看和修改当前无线频率。

## 参数

- `SIDE`: 侧面名称或数值侧面常量，例如 `front`、`north` 或 `3`。
- `VALUE`: 可选输出值。数字会直接使用；`true`、`on`、`yes` 会映射为启用输出。
- `COLOR`: 捆绑红石颜色名称或数值颜色常量，例如 `lime`。
- `FREQUENCY`: 要查看或设置的无线红石频率数值。

## 选项

- `-b`: 进入捆绑红石模式，针对指定侧面和颜色进行操作。
- `-w`: 读取或设置无线红石的布尔输入输出状态。
- `-f`: 读取或设置当前无线红石频率。

## 示例

```lua
redstone front
```
输出前侧普通红石输入值和输出值。

```lua
redstone north 15
```
把北侧普通红石输出强度设置为 `15`。

```lua
redstone -b north lime 200
```
把北侧捆绑红石的 `lime` 通道设置为 `200`。

```lua
redstone -w on
```
启用无线红石输出，然后打印当前无线输入和输出状态。

```lua
redstone -f 42
```
把无线红石频率切换为 `42`，并输出当前生效频率。

## 备注

- 命令要求当前可见 `redstone` 组件。
- 普通红石输出使用 `0` 到 `15` 的强度范围；捆绑红石输出使用 `0` 到 `255`。
- 如果省略输出值，命令会退化为所选模式下的只读状态查询。
