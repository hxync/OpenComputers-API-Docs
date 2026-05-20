# label

## 概述

按路径或地址读取并修改文件系统标签。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\label.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\label`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\label`

## 语法

```lua
label FILE [STRING]
```
```lua
label -a ADDRESS [STRING]
```

## 用途

查看或修改文件系统标签。既可以通过解析到该挂载内的路径来指定目标，也可以在使用 `-a` 时直接通过文件系统地址指定目标。

## 参数

- `FILE`: 能够解析到目标文件系统挂载上的路径。
- `ADDRESS`: 配合 `-a` 使用的文件系统地址或地址前缀。
- `STRING`: 可选的新标签。省略时只输出当前标签。

## 选项

- `-a`: 把第一个参数当作文件系统地址，而不是路径。

## 示例

```lua
label /home
```
输出挂载在 `/home` 上的文件系统标签。

```lua
label -a 93f test
```
把地址以 `93f` 开头的文件系统标签改成 `test`。
