# cd

## 概述

切换 shell 当前工作目录。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\cd.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\cd`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\cd`

## 语法

```lua
cd [DIRECTORY]
```
```lua
cd -
```

## 用途

修改 shell 当前使用的工作目录，后续相对路径都会基于该目录解析。不带参数时切换到 `HOME`；参数为 `-` 时切换到 `OLDPWD`，并输出切换后的目录。

## 参数

- `DIRECTORY`: 可选，目标目录。省略时使用 `HOME` 中保存的路径。

## 选项

- `--help`: 输出简短的用法说明。

## 示例

```lua
cd /bin
```
把当前工作目录切换到 `/bin`。

```lua
cd ../
```
从当前目录切换到上一级目录。

```lua
cd -
```
切换回 `OLDPWD` 中记录的上一个工作目录。

## 备注

- 如果缺少 `HOME` 或 `OLDPWD` 环境变量，命令会在解析路径前直接失败。
- 切换成功后，命令会把变更前的目录写入 `OLDPWD`。
